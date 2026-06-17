# ADR 0011 – Adapter Layer dentro de Core

## Estado

Aceptado

---

## Contexto

Se requiere una capa intermedia entre API/Web y Application para:

* Eliminar lógica de mapping de la API
* Centralizar transformación de DTOs
* Mejorar testabilidad de boundaries

---

## Decisión

Se incluye el proyecto:

> RuralOps.Adapter dentro de Core

---

## Responsabilidades

* Mapear DTOs → Commands/Queries
* Mapear Responses → DTOs
* Actuar como boundary entre Presentation y Application

---

## Reglas

* No contiene lógica de negocio
* No accede a Infrastructure
* Solo depende de:

  * Application
  * DataTransferObjects

---

## Consecuencias

### Positivas

* API completamente limpia
* Mapping centralizado y testeable
* Mejora separación de responsabilidades

### Negativas

* Core más grande
* Mayor número de proyectos en solución
