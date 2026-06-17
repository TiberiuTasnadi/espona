# BackOffice Strategy - Internal Management & Administration

## Contexto

El BackOffice es la interfaz interna del sistema, accesible **solo para Administradores**. Aquí se concentra toda la gestión, administración y análisis del sistema.

A diferencia de la API (que expone funcionalidad limitada), el BackOffice tiene acceso completo a todas las features.

---

## Filosofía

- **Completitud**: Acceso a CRUD completo de todas las entidades
- **Visibilidad**: Dashboards, reportes, análisis
- **Eficiencia**: Operaciones en lote, exportación rápida
- **Seguridad**: Control de acceso por rol, sin exposición al exterior

---

## Tecnología

- **Framework**: ASP.NET Core Razor Pages
- **Rendering**: Server-side (simplificar, no SPA)
- **Styling**: Bootstrap 5 (rápido, simple)
- **Exportación**: Excel

---

## Estructura de navegación

```
BackOffice
├── Dashboard
│   ├── Visión global
│   ├── KPIs principales
│   └── Alertas
├── Administración
│   ├── Gestión de usuarios
│   ├── Gestión de roles
│   └── Auditoria
├── Explotaciones
│   ├── Listado y CRUD
│   └── Asignación de usuarios
├── Parcelas
│   ├── Listado y CRUD
│   ├── Estados y historial
│   └── Asignación de cultivos
├── Cultivos
│   ├── Catálogo y CRUD
│   ├── Asociación a parcelas
│   └── Historial por parcela
├── Trabajos
│   ├── Listado y filtros avanzados
│   ├── Validación y edición
│   ├── Historial completo
│   └── Exportación a Excel
├── Reportes
│   ├── Por explotación
│   ├── Por parcela
│   ├── Por periodo
│   └── Análisis de productividad
└── Configuración
	├── Parámetros del sistema
	├── Tipos de trabajo
	└── Estados de parcelas
```

---

## 1. Dashboard

### Responsabilidad
Proporcionar visión global del estado del sistema.

### Componentes

#### KPIs Principales
```
┌─────────────────┬─────────────────┬─────────────────┐
│  Explotaciones  │     Parcelas    │    Cultivos     │
│        12       │       156       │       45        │
└─────────────────┴─────────────────┴─────────────────┘

┌─────────────────┬─────────────────┬─────────────────┐
│  Trabajos (mes) │  Operarios      │ Parcelas Activas│
│       1.243     │        28       │       142       │
└─────────────────┴─────────────────┴─────────────────┘
```

#### Gráficos
- **Trabajos por tipo** (últimos 30 días)
- **Actividad por explotación** (timeline)
- **Cultivos por estado** (pie chart)
- **Operarios más activos** (ranking)

#### Alertas
- Parcelas sin cultivo activo
- Usuarios inactivos
- Trabajos sin validar (futura feature)

---

## 2. Administración

### 2.1 Gestión de Usuarios

#### Listado
- Tabla con filtros: activos/inactivos, rol, explotación
- Búsqueda por nombre/email
- Orden por fecha de creación

#### CRUD Usuario
**Campos**:
- Email (único)
- Nombre completo
- Teléfono
- Rol (Administrador, Operario)
- Explotaciones (multi-select)
- Estado (Activo/Inactivo)

**Validaciones**:
- BR-032: Usuario puede tener múltiples explotaciones
- BR-031: Usuario inactivo no puede crear trabajos
- Cambio de rol requiere confirmación

**Acciones**:
- Crear, leer, editar, desactivar
- Cambiar contraseña (self-service + admin)
- Enviar email de bienvenida
- Historial de cambios

#### Auditoría de cambios
- Quién cambió qué y cuándo
- Permite rollback de cambios (futura feature)

---

### 2.2 Control de roles

**Roles permitidos en BackOffice**:
- **Administrador**: Acceso total (solo este rol puede acceder)

---

## 3. Explotaciones

### Listado
- Tabla con: nombre, propietario, nº parcelas, nº operarios
- Búsqueda y ordenamiento
- Estado (activa/inactiva)

### CRUD Explotación
**Campos**:
- Nombre (único)
- Propietario
- Ubicación
- Contacto (email, teléfono)
- Descripción
- Estado

**Acciones**:
- Crear, leer, editar, desactivar
- Ver resumen de datos
- Asignar usuarios
- Exportar historial completo

---

## 4. Parcelas

### Listado
- Tabla: código, nombre, surface, ubicación, estado, cultivo activo
- Filtros: por explotación, por estado, por cultivo
- Búsqueda por código/nombre

### CRUD Parcela
**Campos**:
- Código (único por explotación)
- Nombre
- Superficie (hectáreas)
- Ubicación (GPS o texto)
- Explotación (auto-filled si estamos en contexto)
- Estado (ACTIVA, INACTIVA, EN_DESCANSO, EN_PREPARACIÓN)

**Validaciones**:
- BR-002: Toda parcela debe tener explotación
- BR-004: Coherencia de estado con cultivos
- Código único dentro de explotación

**Acciones**:
- CRUD completo
- Ver cultivos (historial + actual)
- Ver trabajos
- Cambiar estado
- Exportar historial

---

## 5. Cultivos

### Listado por Parcela
- Tabla: tipo, inicio, fin, estado
- Filtros: activos/finalizados
- Timeline visual de cultivos

### CRUD Cultivo
**Campos**:
- Tipo de cultivo (dropdown)
- Parcela
- Fecha inicio
- Fecha fin (opcional si activo)
- Observaciones

**Validaciones**:
- BR-010: Fechas válidas
- BR-011: Un solo cultivo activo por parcela
- BR-001: No poder iniciar cultivo si ya existe uno activo
- BR-012: Cultivos finalizados no se modifican

**Acciones**:
- CRUD cultivo
- Ver trabajos asociados
- Finalizar cultivo activo
- Ver historial

---

## 6. Trabajos (WorkOrders)

### Listado avanzado
**Tabla principal**:
- Usuario, parcela, tipo, fecha, duración, notas
- Búsqueda por: usuario, parcela, rango de fechas, tipo
- Ordenamiento por cualquier columna
- Paginación (50 items/página)

**Filtros**:
- Explotación
- Parcela
- Usuario
- Tipo de trabajo
- Rango de fechas
- Estado (si hay)

### CRUD Trabajo
**Campos** (mismo que API, pero aquí editable):
- Usuario
- Parcela
- Tipo de trabajo
- Fecha
- Hora inicio / Hora fin (o duración)
- Notas
- Ubicación (GPS)

**Validaciones**:
- BR-020 a BR-024: Todas las reglas de negocio
- No permitir editar trabajos históricos (locked después de 30 días)

**Acciones**:
- Ver, editar (reciente), eliminar (solo admin)
- Ver detalles completos
- Ver validaciones de reglas de negocio

---

### Exportación a Excel

#### Botón: "Exportar a Excel"
**Genera archivo con**:
- Hoja 1: Trabajos (con filtros aplicados)
- Hoja 2: Resumen por usuario
- Hoja 2: Resumen por parcela
- Hoja 3: Totales por tipo de trabajo

**Columnas**:
- Usuario
- Parcela
- Tipo
- Fecha
- Hora inicio
- Hora fin
- Duración total
- Notas
- Explotación

**Formato**:
- Headers en bold
- Fechas en formato local
- Duraciones en HH:MM
- Colores para facilitar lectura
- Filtros aplicados en subtítulo

---

## 7. Reportes

### Por explotación
- Total de trabajos
- Usuarios activos
- Parcelas activas
- Cultivos en curso
- Trabajos por tipo
- Comparativa con mes anterior

### Por parcela
- Historial de cultivos
- Todos los trabajos
- Trazabilidad completa
- Timeline visual

### Por periodo (mes/trimestre/año)
- Trabajos realizados
- Horas totales
- Usuarios más activos
- Parcelas más trabajadas
- ROI de recursos (futuro)

---

## 8. Configuración (Admin only)

### Parámetros del sistema
- Nombre de la aplicación
- Logo
- Contacto de soporte
- Timezone

### Tipos de trabajo (catálogo)
- Listar tipos predefinidos
- Crear nuevos tipos (futuro)
- Activar/desactivar

### Estados de parcelas
- Estados disponibles
- Transiciones permitidas (futuro)

---

## Seguridad & Control de acceso

### Roles en BackOffice

**Solo Administradores tienen acceso al BackOffice:**
- **Administrador**
  - Acceso total a todas las features
  - CRUD de todas las entidades
  - Gestión de usuarios
  - Dashboards y reportes
  - Exportaciones
  - Auditoría completa

### Auditoría

- Todos los cambios quedan registrados
- Quién, qué, cuándo
- IP/dispositivo (futuro)

---

## Patrón: MVC con Razor Pages

**Estructura por feature**:
```
Pages/
├── Parcels/
│   ├── Index.cshtml (list)
│   ├── Index.cshtml.cs
│   ├── Create.cshtml
│   ├── Create.cshtml.cs
│   ├── Edit.cshtml
│   └── Edit.cshtml.cs
├── Cultivos/
├── WorkOrders/
├── Reports/
└── ...
```

---

## UX/UI Principles

- **Claridad**: Información clara y accesible
- **Eficiencia**: Operaciones rápidas, sin navegación innecesaria
- **Validación**: Feedback inmediato de errores
- **Confirmation**: Acciones destructivas requieren confirmación

---

## Performance

- Paginación en listados
- Índices en búsquedas frecuentes
- Caché de catálogos (tipos de trabajo, etc)
- Lazy loading en reportes

---

## Testing

- Unit tests: PageModels con lógica
- Integration tests: Flujos CRUD
- UI tests: Validaciones y errores
- Load tests: Reportes en lote

---

## Roadmap/Workflow (post-MVP)

- Validación de trabajos (admin aprueba/rechaza)
- Notificaciones (email de cambios)
- Importación en lote desde Excel
- Visualización de mapas (Google Maps API)
- Análisis de productividad avanzado