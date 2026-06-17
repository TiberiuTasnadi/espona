# ADR 0003 – FluentValidation

## Estado

Aceptado

---

## Contexto

Se necesita un mecanismo consistente para validar inputs de casos de uso.

---

## Decisión

Se utiliza FluentValidation en Application Layer para validar Commands y Queries.

---

## Alcance

* Validación de formato
* Validación básica de reglas de entrada
* Prevalidación antes de llegar al dominio

---

## Consecuencias

### Positivas

* Validaciones limpias y reutilizables
* Separación de lógica de validación
* Mejor legibilidad de casos de uso

### Negativas

* Doble capa de validación (Application + Domain)
* Dependencia externa

---

## Nota importante

FluentValidation NO reemplaza reglas de dominio.
