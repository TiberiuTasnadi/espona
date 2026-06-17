# Business Rules (Domain Rules)

## Visión general

Este documento define las reglas de negocio inmutables del sistema RuralOps.

Estas reglas deben cumplirse independientemente de la interfaz, infraestructura o caso de uso.

---

# Reglas de Parcelas

## BR-001: Unicidad de cultivo activo

Una parcela solo puede tener un cultivo activo en un momento dado.

### Implicaciones

* No se puede iniciar un cultivo si ya existe uno activo
* El cultivo anterior debe estar finalizado antes de iniciar uno nuevo

---

## BR-002: Parcelas obligatoriamente asociadas a una explotación

Toda parcela debe pertenecer a una única explotación.

---

## BR-003: Parcelas inactivas

No se pueden registrar trabajos en parcelas inactivas.

---

## BR-004: Integridad del estado de parcela

El estado de la parcela debe ser coherente con su actividad:

* Si tiene cultivo activo → no puede estar en estado "inactiva"
* Si no tiene cultivo → puede estar en descanso o preparación

---

# Reglas de Cultivos

## BR-010: Fechas válidas de cultivo

* La fecha de inicio es obligatoria
* La fecha de fin debe ser posterior a la fecha de inicio
* La fecha de fin es opcional si el cultivo está activo

---

## BR-011: Un solo cultivo activo

No puede existir más de un cultivo activo por parcela.

---

## BR-012: Consistencia histórica

Un cultivo finalizado no puede ser modificado en su periodo activo original.

---

# Reglas de Trabajos (WorkOrders)

## BR-020: Obligación de parcela

Todo trabajo debe estar asociado a una parcela existente.

---

## BR-021: Usuario obligatorio

Todo trabajo debe tener un usuario responsable.

---

## BR-022: Usuario activo

Solo usuarios activos pueden registrar trabajos.

---

## BR-023: Parcelas activas

No se pueden registrar trabajos en parcelas inactivas.

---

## BR-024: Fecha de trabajo

* La fecha del trabajo es obligatoria
* No puede ser futura (salvo configuración explícita del sistema)

---

# Reglas de Usuarios

## BR-030: Estado de usuario

Un usuario puede estar:

* Activo
* Inactivo

---

## BR-031: Restricción de usuario inactivo

Un usuario inactivo:

* No puede crear trabajos
* No puede modificar datos del sistema

---

## BR-032: Asociación a explotación

Un usuario puede pertenecer a una o varias explotaciones.

---

# Reglas de Explotación

## BR-040: Integridad de explotación

Una explotación debe tener al menos:

* Un nombre válido
* Un identificador único

---

## BR-041: Propiedad de datos

Todos los datos (parcelas, cultivos, trabajos) deben estar siempre asociados a una explotación.

---

# Reglas de consistencia global

## BR-050: Trazabilidad obligatoria

Toda acción relevante en el sistema debe quedar registrada como WorkOrder o entidad equivalente.

---

## BR-051: No existencia de datos huérfanos

No pueden existir:

* Parcelas sin explotación
* Cultivos sin parcela
* WorkOrders sin parcela o usuario

---

## BR-052: Inmutabilidad histórica

Los datos históricos de cultivos y trabajos no deben ser eliminados, solo cerrados o desactivados.

---

# Reglas de negocio futuras (NO MVP)

* Automatización de riego
* Integración meteorológica
* Predicción de cultivos
* Optimización de recursos
* Alertas inteligentes

---

# Notas de diseño

Estas reglas deben implementarse en:

* Domain Layer (entidades y value objects)
* Domain Services (si aplica)
* Validaciones previas en Application Layer

Nunca deben depender de:

* API
* Base de datos
* UI
* Frameworks

---

# Objetivo

Garantizar que el sistema:

* Mantenga coherencia de datos
* Evite estados inválidos
* Sea robusto ante cambios futuros
