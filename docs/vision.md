# Scope Definition

## MVP

El MVP tiene como objetivo validar que la aplicación permite gestionar la información básica de una explotación agrícola.

---

# Módulo de parcelas

## Incluye

* Alta de parcela
* Modificación de parcela
* Baja lógica de parcela
* Consulta de parcelas
* Detalle de parcela

## Información mínima

* Código
* Nombre
* Superficie
* Ubicación
* Estado

---

# Módulo de cultivos

## Incluye

* Alta de cultivo
* Modificación de cultivo
* Consulta de cultivo activo
* Histórico de cultivos

## Información mínima

* Tipo de cultivo
* Fecha de inicio
* Fecha de finalización

---

# Módulo de trabajos

## Incluye

* Registrar trabajo realizado
* Consultar trabajos
* Filtrar por parcela
* Filtrar por fechas

## Información mínima

* Tipo de trabajo
* Fecha
* Observaciones
* Usuario responsable

---

# Módulo de usuarios

## Incluye

* Alta de usuario
* Modificación de usuario
* Activación
* Desactivación

## Roles iniciales

* Administrador
* Operario

---

# No incluido

## Gestión económica

* Facturas
* Presupuestos
* Cobros
* Pagos

## Gestión comercial

* Clientes
* Proveedores

## Gestión logística

* Inventario
* Compras
* Almacén

## Automatizaciones

* IA
* Sensores
* Integraciones externas

---

---

# Estrategia de Presentación (Presentation Layer)

## Introducción

El sistema separa la lógica de negocio (Domain + Application) de sus consumidores. La Presentation Layer se divide en dos canales independientes que comparten el mismo núcleo:

1. **API REST** - Consumidor externo (app móvil de operarios)
2. **BackOffice Web** - Interfaz interna (administración completa)

Ambos respetan Clean Architecture y no contaminan el dominio.

---

## 1. API REST (Consumidor Externo)

### Responsabilidad
Exponer funcionalidad limitada y controlada para consumidores externos (app móvil).

### Roles permitidos
- **Operario**: Acceso a endpoints de trabajo (registro, consulta de propios trabajos)
- **Administrador**: Acceso completo (igual que Operario + capacidad de consultar datos de otros usuarios)

### MVP - Endpoints autorizados
- **POST `/api/v1/workorders`** - Registro de trabajo en campo
- **GET `/api/v1/workorders`** - Historial de trabajos del usuario
- **GET `/api/v1/workorders/{id}`** - Detalles de trabajo
- **GET `/api/v1/parcels`** - Listado de parcelas asignadas
- **GET `/api/v1/parcels/{id}`** - Detalles de parcela
- **POST `/api/v1/auth/login`** - Autenticación

### Filosofía
- **ISP (Interface Segregation)**: Expone solo lo necesario para el caso de uso
- **Seguridad**: Autorización por usuario y explotación
- **Escalabilidad**: Preparada para futuras features sin exponerlas
- **DTOs**: Mapeo estricto entre dominio y consumidor

### Consumidor principal
- **Pau (Operario)**: Registra trabajos desde app móvil
- **Administrador**: También puede acceder (para admin de operarios en campo)

### NO incluido (protegido en BackOffice)
- CRUD de parcelas, cultivos, usuarios
- Dashboards y reportes
- Exportación de datos
- Gestión de permisos

**Ver**: [api-strategy.md](./api-strategy.md)

---

## 2. BackOffice Web (Interfaz Interna)

### Responsabilidad
Proporcionar gestión completa del sistema, accesible **solo para Administradores**.

### Tecnología
- **Framework**: ASP.NET Core Razor Pages
- **Rendering**: Server-side (simplicidad)
- **Styling**: Bootstrap 5
- **Exportación**: Excel

### Features principales
- **Dashboard**: KPIs, gráficos, alertas
- **CRUD completo**: Explotaciones, parcelas, cultivos, usuarios, trabajos
- **Reportes**: Por explotación, parcela, período
- **Exportación**: Trabajos a Excel con formatos
- **Auditoría**: Registro de todos los cambios
- **Administración**: Gestión de usuarios y roles

### Consumidores
- **Administrador**: Acceso total a todas las features

### SRP (Single Responsibility)
- Cada PageModel responsable de una acción específica
- Separación clara: Controllers → Services → Domain

**Ver**: [backoffice-strategy.md](./backoffice-strategy.md)

### Beneficios
- **Independencia**: Cambios en API no afectan BackOffice
- **Testabilidad**: Domain + Application sin dependencias UI
- **Seguridad**: API expone poco, BackOffice lo controla todo internamente
- **Escalabilidad**: Fácil agregar nuevos canales (desktop app, CLI, etc)
- **Mantenibilidad**: Capas claras, responsabilidades definidas

---

# Restricciones

El sistema debe priorizar:

* Simplicidad
* Facilidad de mantenimiento
* Bajo coste operativo
* Facilidad de aprendizaje

Las decisiones técnicas deberán alinearse con estos principios.
