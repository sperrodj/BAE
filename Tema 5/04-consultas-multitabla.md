# Bloque 4. Consultas multitabla

## 1. Por qué combinar relaciones

La normalización distribuye hechos entre relaciones. Un `JOIN` permite reconstruir la información necesaria sin volver a almacenarla de forma redundante.

## 2. Producto cartesiano

```sql
SELECT *
FROM cliente
CROSS JOIN categoria;
```

Combina cada cliente con cada categoría. Si hay 100 y 20 filas, genera 2.000 combinaciones.

La sintaxis antigua con comas también produce un producto cartesiano:

```sql
FROM cliente, categoria
```

Preferimos `CROSS JOIN` cuando el producto es intencionado y `JOIN ... ON` cuando existe una condición.

## 3. `INNER JOIN`

Conserva las parejas que cumplen la condición:

```sql
SELECT
    p.id_pedido,
    p.fecha,
    c.nombre AS cliente
FROM pedido AS p
JOIN cliente AS c
  ON c.id_cliente = p.id_cliente;
```

La condición no tiene que usar siempre PK/FK, pero debe expresar la regla correcta y emplear dominios compatibles.

### Varias tablas

```sql
SELECT
    p.id_pedido,
    pr.nombre AS producto,
    lp.cantidad,
    lp.precio_unitario
FROM pedido AS p
JOIN linea_pedido AS lp
  ON lp.id_pedido = p.id_pedido
JOIN producto AS pr
  ON pr.id_producto = lp.id_producto;
```

## 4. Alias y columnas ambiguas

Si varias tablas tienen `nombre`, califica la columna:

```sql
SELECT c.nombre, pr.nombre
```

Usa alias que permitan distinguir conceptos, no letras arbitrarias en consultas grandes.

## 5. `LEFT JOIN`

Conserva todas las filas de la relación izquierda y completa con nulos cuando no hay coincidencia.

```sql
SELECT
    c.id_cliente,
    c.nombre,
    p.id_pedido
FROM cliente AS c
LEFT JOIN pedido AS p
  ON p.id_cliente = c.id_cliente;
```

Los clientes sin pedidos aparecen con `p.id_pedido` nulo.

### Error: convertir un `LEFT JOIN` en interno

```sql
-- Pierde clientes sin pedidos
FROM cliente AS c
LEFT JOIN pedido AS p
  ON p.id_cliente = c.id_cliente
WHERE p.estado = 'pagado';
```

La condición en `WHERE` rechaza las filas extendidas con nulos. Si queremos todos los clientes y únicamente sus pedidos pagados:

```sql
FROM cliente AS c
LEFT JOIN pedido AS p
  ON p.id_cliente = c.id_cliente
 AND p.estado = 'pagado';
```

## 6. `RIGHT JOIN` y `FULL OUTER JOIN`

- `RIGHT JOIN` conserva todas las filas de la derecha.
- `FULL OUTER JOIN` conserva las de ambos lados.

Las versiones modernas de SQLite admiten ambos, pero en código portable suele bastar con reordenar un `RIGHT JOIN` como `LEFT JOIN`. Comprueba siempre la versión del entorno.

## 7. `ON`, `USING` y `NATURAL JOIN`

```sql
FROM pedido
JOIN cliente USING (id_cliente)
```

`USING` es conciso cuando la columna tiene el mismo nombre y significado.

`NATURAL JOIN` combina automáticamente todas las columnas con el mismo nombre. Es frágil: añadir una columna homónima puede cambiar la consulta sin modificar su texto. Se recomienda evitarlo en código mantenible.

## 8. Autocombinación

Si una tabla referencia a sí misma, usa dos alias:

```sql
SELECT
    e.nombre AS empleado,
    s.nombre AS supervisor
FROM empleado AS e
LEFT JOIN empleado AS s
  ON s.id_empleado = e.id_supervisor;
```

## 9. Semijoin con `EXISTS`

Clientes que tienen al menos un pedido:

```sql
SELECT c.id_cliente, c.nombre
FROM cliente AS c
WHERE EXISTS (
    SELECT 1
    FROM pedido AS p
    WHERE p.id_cliente = c.id_cliente
);
```

Evita multiplicar clientes por pedidos y expresa directamente «existe».

## 10. Antijoin

Clientes sin pedidos:

```sql
SELECT c.id_cliente, c.nombre
FROM cliente AS c
WHERE NOT EXISTS (
    SELECT 1
    FROM pedido AS p
    WHERE p.id_cliente = c.id_cliente
);
```

También puede escribirse con `LEFT JOIN ... WHERE p.id_pedido IS NULL`, pero `NOT EXISTS` suele expresar mejor la intención y evita problemas de `NOT IN` con nulos.

## 11. Diagnóstico de duplicados

Antes de añadir `DISTINCT`, pregunta:

- ¿La relación es `1:N` y espero varias filas?
- ¿Falta parte de una clave compuesta en `ON`?
- ¿He combinado dos tablas mediante una columna no única?
- ¿Solo necesito comprobar existencia?

`DISTINCT` puede ocultar el síntoma sin arreglar la condición.

## 12. Ejercicios

1. Lista pedidos con el nombre del cliente.
2. Lista líneas con pedido, producto e importe.
3. Muestra todos los clientes, tengan o no pedidos.
4. Obtén clientes sin pedidos mediante dos técnicas.
5. Muestra todos los productos y las veces que aparecen en pedidos, incluyendo cero.
6. Explica la diferencia entre filtrar en `ON` y en `WHERE` tras un `LEFT JOIN`.

