# Architecture – RuralOps

## Visión general

RuralOps está diseñado siguiendo los principios de Clean Architecture, con una separación estricta entre el dominio del negocio, la orquestación de casos de uso y las capas externas.

El objetivo es garantizar:

- Independencia de frameworks
- Alta testabilidad
- Mantenibilidad a largo plazo
- Evolución del sistema sin impacto en el core
- Aislamiento total de contratos externos

---

# Capas del sistema

## 1. Domain Layer

### Responsabilidad

Contiene el núcleo del negocio y las reglas inmutables del sistema.

### Incluye:

- Entidades
- Value Objects
- Domain Services (si aplica)
- Reglas de negocio
- Interfaces de repositorio (contratos)

### No incluye:

- DTOs
- EF Core
- Servicios externos
- Lógica de aplicación

---

## 2. Application Layer

### Responsabilidad

Orquestación de los casos de uso del sistema.

Implementa la lógica de aplicación sin depender de frameworks externos ni de la capa de presentación.

### Implementación:

- MediatR (Commands / Queries)
- Handlers (Use Cases)
- FluentValidation
- Modelos de aplicación (internos al caso de uso)

### Características:

- No conoce la API
- No conoce DTOs externos
- No conoce la base de datos
- Solo depende del Domain

---

## 3. Adapter Layer

### Responsabilidad

Capa de traducción entre el mundo externo y la Application Layer.

Actúa como boundary explícito del sistema.

### Funciones:

- Mapear DTOs → Commands / Queries
- Mapear Responses → DTOs
- Aislar Application de contratos externos
- Centralizar lógica de transformación

### Restricciones:

- No contiene lógica de negocio
- No accede a Infrastructure
- Solo depende de:
  - Application
  - DataTransferObjects

---

## 4. Infrastructure Layer

### Responsabilidad

Implementación de dependencias externas.

### Incluye:

- Entity Framework Core DbContext
- Configuración Fluent API
- Repositorios concretos
- Servicios externos (APIs, integraciones)

---

## 5. Presentation Layer

### Responsabilidad

Exposición del sistema al exterior.

Contiene únicamente lógica de entrada/salida.

### Incluye:

- Controllers (API)
- Razor Pages / UI (futuro Web)
- Configuración HTTP
- Invocación del Adapter

### No incluye:

- Mapping
- Lógica de negocio
- Acceso a Application directo

---

## 6. DataTransferObjects (Boundaries)

### Responsabilidad

Definir los contratos externos del sistema.

Sirve como punto común entre:

- API
- Adapter
- Web (MVVM futuro)

### Incluye:

- Request DTOs
- Response DTOs

### Restricciones:

- No contiene lógica
- No depende de ninguna otra capa del sistema

---

## 7. Shared Layer

### Responsabilidad

Código transversal reutilizable.

### Incluye:

- Configuración de estilo (StyleCop)
- Analyzer rules
- Constants globales
- Helpers genéricos muy básicos

---

## 8. Tests

### Responsabilidad

Validación del sistema en todos sus niveles.

### Tipos:

- Unit Tests (Domain / Application)
- Integration Tests (Api)

---

# Flujo de datos

Presentation → Adapter → Application → Domain → Application → Adapter → Presentation

---

# Decisiones arquitectónicas (ADRs)

- Uso de Clean Architecture
- Uso de MediatR para CQRS ligero
- Uso de FluentValidation en Application Layer
- Uso de AutoMapper en boundary layers
- Uso de entity framework core en Infrastructure Layer
- Cada modulo tiene sus propios modelos (DTOs, Application Models, Domain Entities)
- Introducción de Adapter Layer como boundary explícito
- Separación de DataTransferObjects como módulo independiente

---

# Principios del sistema

- El dominio es completamente independiente
- Application no depende de infraestructura ni contratos externos
- Presentation es una capa delgada sin lógica
- Adapter actúa como frontera controlada del sistema
- Los contratos externos están completamente aislados

---

# Objetivo del sistema

Construir un sistema:

- Mantenible
- Escalable
- Testable en todos los niveles
- Independiente de frameworks
- Preparado para múltiples presentations (API + Web MVVM)
- Con boundaries explícitos y controlados
