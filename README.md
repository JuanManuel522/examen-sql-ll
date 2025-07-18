# examen-sql-ll
# 🎬 Sistema de Gestión de Alquiler de Películas – SQL

Este proyecto implementa una solución completa de base de datos para un sistema de alquiler de películas, inspirado en el esquema de datos Sakila. Incluye consultas analíticas, funciones, triggers y eventos que permiten obtener métricas clave del negocio, automatizar procesos y garantizar integridad de datos.

---

## 📌 Objetivos

- Consultar estadísticas de clientes, películas y empleados.
- Generar reportes de ingresos y tendencias.
- Automatizar procesos con triggers y eventos.
- Controlar el historial y la auditoría del sistema.

---

## 🧰 Requisitos

- MySQL 8.0+
- Cliente SQL (Workbench, DBeaver o terminal)
- Esquema Sakila (u otro compatible con tablas como: `rental`, `film`, `customer`, `payment`, etc.)

---

## 📁 Estructura del Proyecto

### 🔎 Consultas SQL

Incluye reportes como:

- Cliente con más alquileres en los últimos 6 meses.
- Top 5 películas más alquiladas del año.
- Ingresos y cantidad de alquileres por categoría.
- Número de clientes por idioma en un mes.
- Clientes que alquilaron todas las películas de una categoría.
- Ciudades con más clientes activos.
- Categorías con menos alquileres.
- Tiempo promedio de devolución.
- Empleados con más alquileres en Acción.
- Informe de clientes frecuentes.
- Costo promedio por idioma.
- Películas más largas alquiladas.
- Clientes más activos en Comedia.
- Días totales de alquiler por cliente.
- Alquileres diarios por almacén.
- Ingresos por almacén.
- Alquiler más caro del año.
- Categorías con más ingresos.
- Alquileres por idioma en el mes.
- Clientes inactivos en el último año.

### ⚙️ Funciones SQL

```sql
TotalIngresosCliente(ClienteID, Año)
PromedioDuracionAlquiler(PeliculaID)
IngresosPorCategoria(CategoriaID)
DescuentoFrecuenciaCliente(ClienteID)
EsClienteVIP(ClienteID)
