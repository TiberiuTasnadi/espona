# Scope Definition

## MVP

El MVP tiene como objetivo validar que la aplicación permite gestionar la información básica de una explotación agrícola.

---

# Módulo de parcelas

## Incluye

* Alta de parcela
* Modificación de parcela
* Baja lógica de parcela
* Consulta de parcelas
* Detalle de parcela

## Información mínima

* Código
* Nombre
* Superficie
* Ubicación
* Estado

---

# Módulo de cultivos

## Incluye

* Alta de cultivo
* Modificación de cultivo
* Consulta de cultivo activo
* Histórico de cultivos

## Información mínima

* Tipo de cultivo
* Fecha de inicio
* Fecha de finalización

---

# Módulo de trabajos

## Incluye

* Registrar trabajo realizado
* Consultar trabajos
* Filtrar por parcela
* Filtrar por fechas

## Información mínima

* Tipo de trabajo
* Fecha
* Observaciones
* Usuario responsable

---

# Módulo de usuarios

## Incluye

* Alta de usuario
* Modificación de usuario
* Activación
* Desactivación

## Roles iniciales

* Administrador
* Operario

---

# No incluido

## Gestión económica

* Facturas
* Presupuestos
* Cobros
* Pagos

## Gestión comercial

* Clientes
* Proveedores

## Gestión logística

* Inventario
* Compras
* Almacén

## Automatizaciones

* IA
* Sensores
* Integraciones externas

---

# Restricciones

El sistema debe priorizar:

* Simplicidad
* Facilidad de mantenimiento
* Bajo coste operativo
* Facilidad de aprendizaje

Las decisiones técnicas deberán alinearse con estos principios.
