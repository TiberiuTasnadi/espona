# Use Cases (Application Layer)

## Visión general

Los casos de uso representan las acciones que el sistema puede realizar desde el punto de vista funcional.

Cada caso de uso encapsula una operación completa del sistema, independiente de la interfaz (API, web, CLI).

---

# Principios

* Cada caso de uso hace **una sola cosa**
* No contiene lógica de infraestructura
* Es independiente de la UI
* Trabaja sobre el dominio
* Es testeable de forma aislada

---

# Gestión de Explotaciones (Farm)

## CreateFarm

### Descripción

Crear una nueva explotación agrícola.

### Entradas

* Nombre
* Descripción opcional

### Salidas

* Identificador de la explotación creada

---

## GetFarmById

### Descripción

Obtener información de una explotación.

### Salidas

* Datos básicos de la explotación
* Parcelas asociadas
* Usuarios asociados

---

# Gestión de Parcelas

## CreateParcel

### Descripción

Crear una nueva parcela dentro de una explotación.

### Entradas

* FarmId
* Nombre
* Superficie
* Ubicación

### Reglas aplicadas

* La explotación debe existir
* El nombre de la parcela debe ser único dentro de la explotación

---

## UpdateParcel

### Descripción

Actualizar datos de una parcela.

### Entradas

* ParcelId
* Datos modificables

---

## GetParcelById

### Descripción

Obtener detalle de una parcela.

### Salidas

* Información de la parcela
* Cultivo actual
* Historial de trabajos

---

## GetFarmParcels

### Descripción

Listar todas las parcelas de una explotación.

### Salidas

* Lista de parcelas con estado actual

---

# Gestión de Cultivos

## StartCrop

### Descripción

Iniciar un nuevo cultivo en una parcela.

### Entradas

* ParcelId
* Tipo de cultivo
* Fecha de inicio

### Reglas aplicadas

* La parcela debe existir
* No puede haber otro cultivo activo en la parcela
* El cultivo anterior debe estar cerrado si existe

---

## EndCrop

### Descripción

Finalizar un cultivo activo.

### Entradas

* ParcelId
* Fecha de fin

### Reglas aplicadas

* Debe existir un cultivo activo
* La fecha de fin debe ser posterior a la de inicio

---

## GetCropHistory

### Descripción

Obtener histórico de cultivos de una parcela.

---

# Gestión de Trabajos

## RegisterWorkOrder

### Descripción

Registrar un trabajo realizado en una parcela.

### Entradas

* ParcelId
* UserId
* Tipo de trabajo
* Fecha
* Observaciones

### Reglas aplicadas

* La parcela debe existir y estar activa
* El usuario debe estar activo
* El tipo de trabajo debe ser válido

---

## GetWorkOrdersByParcel

### Descripción

Obtener trabajos realizados en una parcela.

---

## GetWorkOrdersByFarm

### Descripción

Obtener trabajos realizados en toda una explotación.

---

# Gestión de Usuarios

## CreateUser

### Descripción

Crear un usuario del sistema.

---

## DisableUser

### Descripción

Desactivar un usuario.

---

## EnableUser

### Descripción

Activar un usuario previamente desactivado.

---

## GetUsersByFarm

### Descripción

Obtener usuarios asociados a una explotación.

---

# Dashboard / Consulta global

## GetFarmDashboard

### Descripción

Obtener una vista global del estado de una explotación.

### Salidas

* Número de parcelas activas
* Cultivos activos
* Trabajos recientes
* Estado general

---

# Notas de diseño

* Cada caso de uso será una clase independiente en Application Layer
* Cada caso de uso tendrá su propio request/response model
* Se evitará compartir modelos entre casos de uso
* Toda validación de negocio se delega al dominio
* Toda lógica de persistencia se abstrae mediante interfaces

---

# Evolución futura

Posibles ampliaciones:

* AssignUserToFarm
* ScheduleWorkOrder
* BulkWorkOrders
* Reports module
* Export data (CSV / PDF)
