# Domain Model

## Visión general del dominio

RuralOps gestiona explotaciones agrícolas a través de la administración de parcelas, cultivos y trabajos realizados en campo.

El dominio se centra en la trazabilidad de actividades agrícolas y el estado actual de una explotación.

---

# Entidades principales

## Explotación (Farm)

Representa una unidad de gestión agrícola.

### Responsabilidades

* Agrupar parcelas
* Asociar usuarios responsables
* Servir como raíz organizativa del sistema

### Relaciones

* Tiene muchas Parcelas
* Tiene muchos Usuarios asociados

---

## Parcela (Parcel)

Unidad física de terreno dentro de una explotación.

### Responsabilidades

* Representar un terreno agrícola
* Mantener estado actual
* Relacionarse con cultivos y trabajos

### Reglas

* Debe pertenecer a una Explotación
* Solo puede tener un cultivo activo a la vez

### Relaciones

* Pertenece a Farm
* Tiene uno o varios Cultivos (histórico)
* Tiene muchos WorkOrders

---

## Cultivo (Crop)

Representa un tipo de cultivo en una parcela durante un periodo de tiempo.

### Responsabilidades

* Registrar qué se cultiva en una parcela
* Mantener historial agrícola

### Reglas

* Solo un cultivo activo por parcela
* Debe tener fecha de inicio
* Puede tener fecha de fin (opcional si está activo)

### Relaciones

* Pertenece a Parcel

---

## Trabajo (WorkOrder)

Acción realizada sobre una parcela.

Ejemplos:

* Riego
* Fertilización
* Cosecha
* Preparación del terreno

### Responsabilidades

* Registrar actividad agrícola
* Mantener trazabilidad de acciones

### Reglas

* Debe estar asociado a una Parcela
* Debe tener fecha de ejecución
* Debe tener un tipo de trabajo
* Debe tener un usuario responsable

### Relaciones

* Pertenece a Parcel
* Pertenece a User

---

## Usuario (User)

Persona que interactúa con el sistema.

### Responsabilidades

* Ejecutar trabajos
* Gestionar explotaciones (según rol)

### Reglas

* Puede estar activo o inactivo
* No puede realizar acciones si está deshabilitado

### Relaciones

* Puede pertenecer a una o varias Farms
* Puede crear WorkOrders

---

# Value Objects

## Tipo de Trabajo (WorkType)

Representa la naturaleza del trabajo realizado.

Ejemplos:

* Riego
* Fertilización
* Poda
* Siembra
* Cosecha

---

## Estado de Parcela

Representa el estado actual de una parcela.

Ejemplos:

* Activa
* En descanso
* Preparación
* Inactiva

---

# Reglas globales del dominio

* Una Parcela no puede tener más de un Cultivo activo.
* Todo WorkOrder debe estar asociado a una Parcela.
* Todo WorkOrder debe tener usuario responsable.
* No se pueden registrar WorkOrders en parcelas inactivas.
* Un Usuario inactivo no puede generar WorkOrders.
* Todo cultivo debe tener fecha de inicio válida.

---

# Observaciones del modelo

Este modelo está diseñado para ser:

* Simple pero extensible
* Independiente de infraestructura
* Compatible con Clean Architecture
* Preparado para DDD ligero (sin sobre-ingeniería)

---

# Evolución futura del dominio (NO MVP)

* Sensores IoT en parcelas
* Automatización de riegos
* Predicción de cultivos
* Gestión de maquinaria
* Integración meteorológica
