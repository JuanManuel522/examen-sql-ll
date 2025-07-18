```sql

-- CONSULTAS SQL



-- 1. Cliente con más alquileres últimos 6 meses
SELECT c.id_cliente, c.nombre, c.apellidos, COUNT(*) AS cantidad_alquileres
FROM alquiler a
JOIN cliente c ON a.id_cliente = c.id_cliente
WHERE a.fecha_alquiler >= DATE_SUB(CURDATE(), INTERVAL 6 MONTH)
GROUP BY c.id_cliente
ORDER BY cantidad_alquileres DESC
LIMIT 1;


-- 2. 5 películas más alquiladas último año
SELECT p.titulo, COUNT(*) AS total_alquileres
FROM alquiler a
JOIN inventario i ON a.id_inventario = i.id_inventario
JOIN pelicula p ON i.id_pelicula = p.id_pelicula
WHERE a.fecha_alquiler >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
GROUP BY p.id_pelicula
ORDER BY total_alquileres DESC
LIMIT 5;


-- 3. Ingresos y cantidad por categoría
SELECT cat.nombre AS categoria, COUNT(*) AS cantidad_alquileres, SUM(p.total) AS ingresos
FROM categoria cat
JOIN pelicula_categoria pc ON cat.id_categoria = pc.id_categoria
JOIN inventario i ON pc.id_pelicula = i.id_pelicula
JOIN alquiler a ON i.id_inventario = a.id_inventario
JOIN pago p ON a.id_alquiler = p.id_alquiler
GROUP BY cat.id_categoria;


-- 4. Total de clientes por idioma (ej. junio 2025)
SELECT i.nombre AS idioma, COUNT(DISTINCT a.id_cliente) AS total_clientes
FROM alquiler a
JOIN inventario inv ON a.id_inventario = inv.id_inventario
JOIN pelicula p ON inv.id_pelicula = p.id_pelicula
JOIN idioma i ON p.id_idioma = i.id_idioma
WHERE MONTH(a.fecha_alquiler) = 6 AND YEAR(a.fecha_alquiler) = 2025
GROUP BY i.id_idioma;


-- 5. Clientes que alquilaron todas las películas de una misma categoría
SELECT c.id_cliente, c.nombre, c.apellidos, cat.nombre AS categoria
FROM cliente c
JOIN alquiler a ON c.id_cliente = a.id_cliente
JOIN inventario i ON a.id_inventario = i.id_inventario
JOIN pelicula p ON i.id_pelicula = p.id_pelicula
JOIN pelicula_categoria pc ON p.id_pelicula = pc.id_pelicula
JOIN categoria cat ON pc.id_categoria = cat.id_categoria
GROUP BY c.id_cliente, cat.id_categoria
HAVING COUNT(DISTINCT p.id_pelicula) = (
    SELECT COUNT(*) FROM pelicula_categoria WHERE id_categoria = cat.id_categoria
);


-- 6. 3 ciudades con más clientes activos último trimestre
SELECT ci.nombre, COUNT(*) AS total_clientes
FROM cliente c
JOIN direccion d ON c.id_direccion = d.id_direccion
JOIN ciudad ci ON d.id_ciudad = ci.id_ciudad
WHERE c.activo = 1 AND c.fecha_creacion >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH)
GROUP BY ci.id_ciudad
ORDER BY total_clientes DESC
LIMIT 3;


-- 7. 5 categorías con menos alquileres último año
SELECT cat.nombre, COUNT(*) AS total
FROM categoria cat
JOIN pelicula_categoria pc ON cat.id_categoria = pc.id_categoria
JOIN inventario i ON pc.id_pelicula = i.id_pelicula
JOIN alquiler a ON i.id_inventario = a.id_inventario
WHERE a.fecha_alquiler >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
GROUP BY cat.id_categoria
ORDER BY total ASC
LIMIT 5;


-- 8. Promedio de días en devolver películas
SELECT AVG(DATEDIFF(fecha_devolucion, fecha_alquiler)) AS promedio_dias
FROM alquiler
WHERE fecha_devolucion IS NOT NULL;


-- 9. 5 empleados con más alquileres en Acción
SELECT e.id_empleado, e.nombre, COUNT(*) AS total
FROM alquiler a
JOIN empleado e ON a.id_empleado = e.id_empleado
JOIN inventario i ON a.id_inventario = i.id_inventario
JOIN pelicula p ON i.id_pelicula = p.id_pelicula
JOIN pelicula_categoria pc ON p.id_pelicula = pc.id_pelicula
JOIN categoria c ON pc.id_categoria = c.id_categoria
WHERE c.nombre = 'Acción'
GROUP BY e.id_empleado
ORDER BY total DESC
LIMIT 5;


-- 10. Informe de clientes con alquileres más frecuentes
SELECT c.id_cliente, c.nombre, c.apellidos, COUNT(*) AS total_alquileres
FROM cliente c
JOIN alquiler a ON c.id_cliente = a.id_cliente
GROUP BY c.id_cliente
ORDER BY total_alquileres DESC;


-- 11. Costo promedio por idioma
SELECT i.nombre, AVG(p.rental_rate) AS costo_promedio
FROM pelicula p
JOIN idioma i ON p.id_idioma = i.id_idioma
GROUP BY i.id_idioma;


-- 12. 5 películas con mayor duración alquiladas último año
SELECT f.titulo, f.duracion
FROM alquiler a
JOIN inventario i ON a.id_inventario = i.id_inventario
JOIN pelicula f ON i.id_pelicula = f.id_pelicula
WHERE a.fecha_alquiler >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
GROUP BY f.id_pelicula
ORDER BY f.duracion DESC
LIMIT 5;


-- 13. Clientes que más alquilaron películas de Comedia
SELECT c.id_cliente, c.nombre, c.apellidos, COUNT(*) AS total
FROM cliente c
JOIN alquiler a ON c.id_cliente = a.id_cliente
JOIN inventario i ON a.id_inventario = i.id_inventario
JOIN pelicula p ON i.id_pelicula = p.id_pelicula
JOIN pelicula_categoria pc ON p.id_pelicula = pc.id_pelicula
JOIN categoria cat ON pc.id_categoria = cat.id_categoria
WHERE cat.nombre = 'Comedia'
GROUP BY c.id_cliente
ORDER BY total DESC;


-- 14. Días alquilados por cliente en último mes
SELECT c.id_cliente, c.nombre, SUM(DATEDIFF(a.fecha_devolucion, a.fecha_alquiler)) AS total_dias
FROM cliente c
JOIN alquiler a ON c.id_cliente = a.id_cliente
WHERE MONTH(a.fecha_alquiler) = MONTH(CURDATE()) - 1
GROUP BY c.id_cliente;


-- 15. Alquileres diarios por almacén (último trimestre)
SELECT a.id_almacen, DATE(alq.fecha_alquiler) AS fecha, COUNT(*) AS total
FROM alquiler alq
JOIN inventario i ON alq.id_inventario = i.id_inventario
JOIN almacen a ON i.id_almacen = a.id_almacen
WHERE alq.fecha_alquiler >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH)
GROUP BY a.id_almacen, fecha;


-- 16. Ingresos por almacén último semestre
SELECT a.id_almacen, SUM(p.total) AS ingresos
FROM pago p
JOIN alquiler alq ON p.id_alquiler = alq.id_alquiler
JOIN inventario i ON alq.id_inventario = i.id_inventario
JOIN almacen a ON i.id_almacen = a.id_almacen
WHERE p.fecha_pago >= DATE_SUB(CURDATE(), INTERVAL 6 MONTH)
GROUP BY a.id_almacen;


-- 17. Alquiler más caro del año
SELECT c.id_cliente, c.nombre, c.apellidos, MAX(p.total) AS alquiler_maximo
FROM pago p
JOIN cliente c ON p.id_cliente = c.id_cliente
WHERE p.fecha_pago >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
GROUP BY c.id_cliente
ORDER BY alquiler_maximo DESC
LIMIT 1;


-- 18. 5 categorías con más ingresos últimos 3 meses
SELECT c.nombre, SUM(p.total) AS ingresos
FROM categoria c
JOIN pelicula_categoria pc ON c.id_categoria = pc.id_categoria
JOIN inventario i ON pc.id_pelicula = i.id_pelicula
JOIN alquiler a ON i.id_inventario = a.id_inventario
JOIN pago p ON a.id_alquiler = p.id_alquiler
WHERE p.fecha_pago >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH)
GROUP BY c.id_categoria
ORDER BY ingresos DESC
LIMIT 5;


-- 19. Películas alquiladas por idioma último mes
SELECT i.nombre AS idioma, COUNT(*) AS cantidad
FROM alquiler a
JOIN inventario i2 ON a.id_inventario = i2.id_inventario
JOIN pelicula p ON i2.id_pelicula = p.id_pelicula
JOIN idioma i ON p.id_idioma = i.id_idioma
WHERE MONTH(a.fecha_alquiler) = MONTH(CURDATE()) - 1
GROUP BY i.id_idioma;


-- 20. Clientes sin alquiler último año
SELECT c.id_cliente, c.nombre, c.apellidos
FROM cliente c
LEFT JOIN alquiler a ON c.id_cliente = a.id_cliente AND a.fecha_alquiler >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
WHERE a.id_alquiler IS NULL;
```

