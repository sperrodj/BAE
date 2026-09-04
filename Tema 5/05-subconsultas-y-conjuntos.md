# Bloque 5. Subconsultas y operaciones de conjuntos

## 1. Qué es una subconsulta

Es una consulta dentro de otra sentencia. Puede aparecer en `WHERE`, `HAVING`, `FROM` o la lista `SELECT`, según el tipo de resultado y el SGBD.

## 2. Subconsulta escalar

Debe devolver una fila y una columna:

```sql
SELECT nombre, precio
FROM producto
WHERE precio > (
    SELECT AVG(precio)
    FROM producto
);
```

Si devuelve varias filas donde se espera un valor, la consulta es errónea o dependiente del comportamiento del producto.

## 3. Subconsulta de varias filas

```sql
SELECT nombre
FROM producto
WHERE id_categoria IN (
    SELECT id_categoria
    FROM categoria
    WHERE nombre IN ('Hardware', 'Accesorios')
);
```

## 4. Subconsulta correlacionada

Hace referencia a la fila de la consulta exterior:

```sql
SELECT c.id_cliente, c.nombre
FROM cliente AS c
WHERE EXISTS (
    SELECT 1
    FROM pedido AS p
    WHERE p.id_cliente = c.id_cliente
      AND p.estado = 'pagado'
);
```

Conceptualmente se evalúa para cada fila exterior, aunque el optimizador puede transformarla.

## 5. `EXISTS` y `NOT EXISTS`

`EXISTS` comprueba si la subconsulta produce alguna fila. El contenido de `SELECT` no importa:

```sql
SELECT 1
```

es una convención legible.

Usa `NOT EXISTS` para expresar «ninguno» y evitar la trampa de nulos de `NOT IN`.

## 6. Comparaciones universales

«Clientes que han comprado todos los productos de una categoría» puede expresarse como «no existe un producto requerido que el cliente no haya comprado»:

```sql
SELECT c.id_cliente, c.nombre
FROM cliente AS c
WHERE NOT EXISTS (
    SELECT 1
    FROM producto AS pr
    JOIN categoria AS ca
      ON ca.id_categoria = pr.id_categoria
    WHERE ca.nombre = 'Hardware'
      AND NOT EXISTS (
          SELECT 1
          FROM pedido AS p
          JOIN linea_pedido AS lp
            ON lp.id_pedido = p.id_pedido
          WHERE p.id_cliente = c.id_cliente
            AND lp.id_producto = pr.id_producto
      )
);
```

Es la traducción práctica de una división relacional.

## 7. Subconsultas en `FROM`

Se comportan como una relación derivada y deben tener alias:

```sql
SELECT resumen.id_cliente, resumen.total
FROM (
    SELECT
        p.id_cliente,
        SUM(lp.cantidad * lp.precio_unitario) AS total
    FROM pedido AS p
    JOIN linea_pedido AS lp
      ON lp.id_pedido = p.id_pedido
    GROUP BY p.id_cliente
) AS resumen
WHERE resumen.total > 500;
```

## 8. Operaciones de conjuntos

### `UNION`

Combina resultados compatibles y elimina duplicados:

```sql
SELECT email FROM cliente
UNION
SELECT email FROM proveedor;
```

### `UNION ALL`

Conserva duplicados y evita el trabajo de eliminarlos. Úsalo cuando los duplicados son correctos o imposibles.

### `INTERSECT`

Devuelve filas presentes en ambos resultados.

### `EXCEPT`

Devuelve filas del primer resultado ausentes en el segundo.

Los lados deben devolver el mismo número de columnas y tipos compatibles por posición.

## 9. Orden en consultas compuestas

`ORDER BY` y `LIMIT` se aplican normalmente al resultado compuesto completo:

```sql
SELECT nombre AS etiqueta FROM cliente
UNION
SELECT nombre FROM producto
ORDER BY etiqueta;
```

## 10. Elegir `JOIN`, subconsulta o conjunto

- `JOIN`: necesitas columnas relacionadas de varias tablas.
- `EXISTS`: preguntas si hay alguna coincidencia.
- `NOT EXISTS`: preguntas si no hay ninguna.
- subconsulta escalar: necesitas comparar con un único valor calculado.
- `UNION ALL`: apilas filas de orígenes compatibles.

La legibilidad y el plan real importan más que reglas absolutas sobre cuál «es más rápido».

## 11. Ejercicios

1. Productos con precio superior a la media.
2. Clientes con al menos un pedido pagado.
3. Clientes sin pedidos cancelados.
4. Categorías sin productos mediante `NOT EXISTS`.
5. Combina nombres de clientes y productos con `UNION`.
6. Obtén productos vendidos en dos periodos mediante `INTERSECT`.

