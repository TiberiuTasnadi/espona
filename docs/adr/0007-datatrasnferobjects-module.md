# ADR 0007 – DataTransferObjects Module

## Estado

Aceptado

---

## Contexto

Se requiere un conjunto de modelos compartidos para la comunicación entre:

* API (Presentation)
* Adapter Layer
* Web MVVM (futuro)

Además, Adapter necesita conocer estos modelos para realizar mapping bidireccional.

---

## Decisión

Se crea un proyecto independiente:

> RuralOps.DataTransferObjects

Este proyecto contiene:

* DTOs de entrada (Requests)
* DTOs de salida (Responses)

---

## Reglas

* No contiene lógica de negocio
* No contiene referencias a Domain o Application
* Puede ser referenciado por:

  * API
  * Adapter
  * Web (futuro)

---

## Justificación

* Evita dependencias circulares
* Permite reutilización en múltiples frontends
* Mantiene API completamente libre de lógica
* Permite Adapter como capa pura de transformación

---

## Consecuencias

### Positivas

* Separación clara de contratos externos
* Reutilización entre API y Web
* Adapter completamente desacoplado de Application

### Negativas

* Más proyectos en la solución
* Duplicación conceptual con Application models
