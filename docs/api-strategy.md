# API Strategy - Mobile & External Consumers

## Contexto

La API REST es el punto de entrada para consumidores externos, inicialmente enfocada en la aplicación móvil de operarios en campo.

La API NO expone toda la funcionalidad del sistema, solo aquella que tiene sentido en contextos móviles y descentralizados.

---

## Filosofía

- **Seguridad**: Autorización por usuario y explotación
- **Simplicidad**: DTOs específicos, no entidades del dominio
- **Escalabilidad**: Preparada para futuras features (sin exponerlas aún)
- **SOLID**: ISP (Interface Segregation Principle) - API expone solo lo necesario

---

## MVP: Endpoints autorizados

Los ejemplos son orientativos, pueden variar según la implementación final. Para ver la documentación completa de cada endpoint, consultar OpenAPI/Swagger.

### 1. Registro de Trabajo (WorkOrder)

#### POST `/api/v1/workorders`

**Responsabilidad**: Crear un nuevo registro de trabajo en campo.

**Consumer**: Pau (operario) desde app móvil

**Request Body**:
```json
{
  "parcelId": "uuid",
  "workType": "RIEGO|COSECHA|LIMPIEZA|MANTENIMIENTO",
  "startTime": "2024-01-15T09:00:00Z",
  "endTime": "2024-01-15T12:30:00Z",
  "notes": "Se realizó riego completo de la parcela",
  "latitude": 41.3851,
  "longitude": 2.1734
}
```

**Response** (201 Created):
```json
{
  "id": "uuid",
  "parcelId": "uuid",
  "workType": "RIEGO",
  "date": "2024-01-15",
  "duration": "03:30",
  "notes": "Se realizó riego completo de la parcela",
  "createdAt": "2024-01-15T12:35:00Z",
  "status": "COMPLETED"
}
```

**Reglas de negocio aplicadas**:
- BR-020: Parcela debe existir
- BR-021: Usuario obligatorio (extraído del token)
- BR-022: Usuario debe estar activo
- BR-023: Parcela debe estar activa
- BR-024: Fecha no puede ser futura
- BR-050: Trazabilidad (se registra automáticamente)

**Errores**:
- `400 Bad Request`: Datos inválidos
- `401 Unauthorized`: Token inválido o expirado
- `403 Forbidden`: Usuario inactivo o sin acceso a explotación
- `404 Not Found`: Parcela no existe
- `409 Conflict`: Parcela inactiva

---

#### GET `/api/v1/workorders`

**Responsabilidad**: Listar trabajos del usuario autenticado en un rango de fechas.

**Parámetros**:
- `from` (query): YYYY-MM-DD
- `to` (query): YYYY-MM-DD
- `parcelId` (query, opcional): Filtrar por parcela

**Response** (200 OK):
```json
{
  "total": 42,
  "items": [
	{
	  "id": "uuid",
	  "parcelId": "uuid",
	  "parcelName": "Parcela Norte",
	  "workType": "RIEGO",
	  "date": "2024-01-15",
	  "duration": "03:30",
	  "createdAt": "2024-01-15T12:35:00Z"
	}
  ]
}
```

**Reglas de negocio aplicadas**:
- Solo ve sus propios trabajos
- Solo ve trabajos en explotaciones donde tiene acceso

---

#### GET `/api/v1/workorders/{id}`

**Responsabilidad**: Obtener detalles de un trabajo específico.

**Response** (200 OK):
```json
{
  "id": "uuid",
  "parcelId": "uuid",
  "parcelName": "Parcela Norte",
  "cultivType": "TOMATE",
  "workType": "RIEGO",
  "date": "2024-01-15",
  "startTime": "09:00",
  "endTime": "12:30",
  "duration": "03:30",
  "notes": "Se realizó riego completo de la parcela",
  "latitude": 41.3851,
  "longitude": 2.1734,
  "createdAt": "2024-01-15T12:35:00Z",
  "createdBy": "Pau García"
}
```

**Errores**:
- `404 Not Found`: Trabajo no existe o sin permisos

---

### 2. Consulta de Parcelas (Lectura)

#### GET `/api/v1/parcels`

**Responsabilidad**: Listar parcelas asignadas al usuario en una explotación.

**Parámetros**:
- `exploitationId` (query): UUID de la explotación

**Response** (200 OK):
```json
{
  "total": 5,
  "items": [
	{
	  "id": "uuid",
	  "code": "P-001",
	  "name": "Parcela Norte",
	  "surface": 2.5,
	  "state": "ACTIVE",
	  "currentCulture": {
		"type": "TOMATE",
		"startDate": "2024-01-01"
	  }
	}
  ]
}
```

**Reglas de negocio aplicadas**:
- Solo ve parcelas de su explotación asignada
- Solo ve parcelas activas

---

#### GET `/api/v1/parcels/{id}`

**Responsabilidad**: Detalles de una parcela para consulta en campo.

**Response** (200 OK):
```json
{
  "id": "uuid",
  "code": "P-001",
  "name": "Parcela Norte",
  "surface": 2.5,
  "location": "Coordenadas GPS",
  "state": "ACTIVE",
  "currentCulture": {
	"type": "TOMATE",
	"startDate": "2024-01-01",
	"expectedEndDate": "2024-05-30"
  },
  "lastWorkOrder": {
	"date": "2024-01-14",
	"type": "RIEGO",
	"notes": "Riego completo"
  }
}
```

---

### 3. Autenticación

#### POST `/api/v1/auth/login`

**Request Body**:
```json
{
  "email": "pau@example.com",
  "password": "secure_password"
}
```

**Response** (200 OK):
```json
{
  "accessToken": "jwt_token",
  "tokenType": "Bearer",
  "expiresIn": 3600,
  "exploitations": [
	{
	  "id": "uuid",
	  "name": "Explotación García"
	}
  ]
}
```

**Errores**:
- `401 Unauthorized`: Credenciales inválidas
- `403 Forbidden`: Usuario inactivo

---

## NO INCLUIDO en MVP (solo en BackOffice)

- ❌ CRUD de parcelas, cultivos, usuarios
- ❌ Dashboards
- ❌ Exportación de datos
- ❌ Reportes
- ❌ Gestión de permisos y roles
- ❌ Auditoría

Estas features están disponibles **solo en BackOffice** para Administradores.

---

## Seguridad & Autorización

### Roles permitidos en API

- **Operario**: Acceso a endpoints de trabajo (registro, consulta de propios trabajos)
- **Administrador**: Acceso completo (igual que Operario + capacidad de consultar datos de otros usuarios)

### Token JWT

- Includes: `userId`, `exploitationId`, `role`
- Expiration: 1 hora
- Refresh: Disponible (no incluido en MVP)

### Validaciones por endpoint

1. **Autenticación**: Token válido y no expirado
2. **Autorización**: 
   - Operarios: Solo acceso a sus propios trabajos
   - Administradores: Acceso completo (pueden ver datos de cualquier usuario dentro de su explotación)
3. **Reglas de negocio**: Aplicadas en Application Layer

### CORS

- Origen permitido: App móvil domain (configurar en deployment)

---

## Versionado de API

- Versión actual: `v1`
- Path: `/api/v1/...`
- Breaking changes: Nueva versión (`v2`)
- Deprecation: 6 meses de aviso

---

## Rate Limiting

- 100 requests por minuto por usuario
- Header: `X-RateLimit-Remaining`

---

## Logging & Trazabilidad

- Toda acción se registra en Application Layer
- IDs de traza distribuida: `X-Correlation-Id`
- Auditoría: Quién, qué, cuándo, desde dónde

---

## Documentación

- OpenAPI/Swagger en `/api/v1/docs`
- Endpoint deprecation explícita
- Ejemplos de curl/Postman