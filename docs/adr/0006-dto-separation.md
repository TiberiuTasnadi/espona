# ADR 0006 – Separación de DTOs por capa

## Estado

Aceptado

---

## Contexto

Se requiere evitar acoplamiento entre API externa y Application Layer.

---

## Decisión

Cada capa tendrá sus propios modelos:

* API DTOs (Presentation Layer)
* Application Models (Application Layer)
* Domain Entities (Domain Layer)

---

## Flujo de datos

API DTO → Application Model → Domain → Application Response → API DTO

---

## Consecuencias

### Positivas

* Independencia entre capas
* Evolución flexible de API
* Mayor control de contratos

### Negativas

* Más mapeo
* Más clases

---

## Justificación

Se prioriza mantenibilidad y evolución del sistema frente a simplicidad inicial.
