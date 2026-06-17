# ADR 0004 – AutoMapper

## Estado

Aceptado

---

## Contexto

Se requiere mapear objetos entre capas:

* API DTOs → Application Models
* Application Responses → API DTOs

---

## Decisión

Se adopta AutoMapper únicamente para mapping entre capas externas y Application Layer.

---

## Reglas de uso

* Permitido: Presentation ↔ Application
* Prohibido: Domain mapping automático interno
* Prohibido: lógica de negocio en profiles

---

## Consecuencias

### Positivas

* Reduce código repetitivo
* Simplifica mapping de DTOs
* Mejora mantenibilidad en API

### Negativas

* Debugging menos explícito
* Posible abuso si no se controla

---

## Alternativas

* Mapping manual
* Extension methods

Se descarta por mayor carga de mantenimiento.
