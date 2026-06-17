# ADR 0002 – MediatR

## Estado

Aceptado

---

## Contexto

Se requiere una forma consistente de implementar casos de uso en Application Layer sin acoplarse a controladores ni infraestructura.

---

## Decisión

Se adopta MediatR para implementar CQRS ligero:

* Commands para escritura
* Queries para lectura
* Handlers como unidades de caso de uso

---

## Consecuencias

### Positivas

* Separación clara de casos de uso
* Fácil testeo de handlers
* Eliminación de lógica en controllers
* Estructura escalable

### Negativas

* Indirección adicional
* Debugging más complejo al principio
* Dependencia de librería externa

---

## Alternativas

* Servicios de aplicación tradicionales
* Controllers con lógica directa

Se descartan por menor escalabilidad.
