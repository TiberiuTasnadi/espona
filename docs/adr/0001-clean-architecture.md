# ADR 0001 – Clean Architecture

## Estado

Aceptado

---

## Contexto

RuralOps es un sistema que debe evolucionar con el tiempo, manteniendo independencia de frameworks, UI y base de datos.

Se requiere una arquitectura que permita:

* Alta testabilidad
* Bajo acoplamiento
* Evolución independiente de la infraestructura

---

## Decisión

Se adopta Clean Architecture como base del sistema.

El sistema se organiza en capas:

* Domain
* Application
* Infrastructure
* Presentation

---

## Consecuencias

### Positivas

* El dominio queda completamente aislado
* Se facilita testing unitario
* Se pueden cambiar tecnologías sin afectar el core
* Código más mantenible

### Negativas

* Mayor complejidad inicial
* Más archivos y estructura
* Curva de aprendizaje más alta

---

## Alternativas consideradas

* Arquitectura en capas tradicional
* Hexagonal Architecture

Se descarta en favor de Clean Architecture por claridad y adopción en ecosistema .NET.
