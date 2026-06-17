# ADR 0005 – Entity Framework Core con Fluent API

## Estado

Aceptado

---

## Contexto

Se requiere persistencia relacional con configuración flexible y separada del dominio.

---

## Decisión

Se adopta Entity Framework Core como ORM.

La configuración se realiza mediante Fluent API en clases separadas por entidad.

---

## Reglas

* Domain NO contiene anotaciones de persistencia
* Configuración en Infrastructure Layer
* DbContext aislado del dominio

---

## Consecuencias

### Positivas

* Separación clara de persistencia
* Alta flexibilidad de configuración
* Código de dominio limpio

### Negativas

* Más configuración inicial
* Más archivos

---

## Alternativas

* Dapper
* SQL manual
* EF con DataAnnotations

Se descartan por menor control o menor separación de capas.
