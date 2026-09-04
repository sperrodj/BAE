# Chuleta de consultas SQL

> Sintaxis orientativa para SQLite. Consulta cada bloque para conocer significado, límites y alternativas.

## Consulta básica

```sql
SELECT columna, expresion AS alias
FROM tabla AS t
WHERE condicion
ORDER BY columna DESC
LIMIT 10 OFFSET 0;
```

## Filtros

```sql
WHERE precio >= 10
  AND estado IN ('pagado', 'enviado')
  AND fecha BETWEEN '2026-01-01' AND '2026-12-31'
  AND nombre LIKE 'A%'
  AND ciudad IS NOT NULL
```

Para fechas con hora:

```sql
WHERE fecha >= '2026-01-01'
  AND fecha <  '2027-01-01'
```

## Condicionales y nulos

```sql
CASE
    WHEN stock = 0 THEN 'agotado'
    WHEN stock < 5 THEN 'bajo'
    ELSE 'disponible'
END

COALESCE(ciudad, 'Sin informar')
```

## Agregación

```sql
SELECT
    id_cliente,
    COUNT(*) AS pedidos,
    SUM(total) AS importe,
    AVG(total) AS media,
    MIN(total) AS minimo,
    MAX(total) AS maximo
FROM resumen_pedido
WHERE estado <> 'cancelado'
GROUP BY id_cliente
HAVING COUNT(*) >= 3;
```

## `INNER JOIN`

```sql
SELECT p.id_pedido, c.nombre
FROM pedido AS p
JOIN cliente AS c
  ON c.id_cliente = p.id_cliente;
```

## `LEFT JOIN`

```sql
SELECT c.nombre, p.id_pedido
FROM cliente AS c
LEFT JOIN pedido AS p
  ON p.id_cliente = c.id_cliente;
```

## Existencia y ausencia

```sql
WHERE EXISTS (
    SELECT 1
    FROM pedido AS p
    WHERE p.id_cliente = c.id_cliente
)

WHERE NOT EXISTS (
    SELECT 1
    FROM pedido AS p
    WHERE p.id_cliente = c.id_cliente
)
```

## Subconsulta escalar

```sql
SELECT nombre, precio
FROM producto
WHERE precio > (SELECT AVG(precio) FROM producto);
```

## Operaciones de conjuntos

```sql
consulta_1
UNION ALL
consulta_2;

consulta_1 INTERSECT consulta_2;
consulta_1 EXCEPT consulta_2;
```

## CTE

```sql
WITH resumen AS (
    SELECT id_cliente, COUNT(*) AS pedidos
    FROM pedido
    GROUP BY id_cliente
)
SELECT *
FROM resumen
WHERE pedidos >= 3;
```

## Función de ventana

```sql
SELECT
    nombre,
    id_categoria,
    precio,
    DENSE_RANK() OVER (
        PARTITION BY id_categoria
        ORDER BY precio DESC
    ) AS posicion
FROM producto;
```

## Patrones en SQLite

```sql
WHERE nombre LIKE 'A%'
WHERE nombre GLOB '[A-C]*'
WHERE texto REGEXP :patron  -- requiere implementación/extensión
```

## Plan de ejecución

```sql
EXPLAIN QUERY PLAN
SELECT *
FROM pedido
WHERE id_cliente = 10;
```

## Orden lógico

```text
FROM/JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT
```

## Antes de entregar

- [ ] ¿Responde exactamente a la pregunta?
- [ ] ¿He contemplado nulos y ausencia de filas?
- [ ] ¿Los duplicados son correctos?
- [ ] ¿Cada `JOIN` contiene la condición completa?
- [ ] ¿El agrupamiento es inequívoco?
- [ ] ¿El orden es estable si uso `LIMIT`?
- [ ] ¿Evito `SELECT *` sin necesidad?
- [ ] ¿He probado casos límite?
- [ ] ¿He revisado el plan si el rendimiento importa?

