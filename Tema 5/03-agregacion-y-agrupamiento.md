# Bloque 3. Agregación y agrupamiento

## 1. Funciones de agregado

Procesan un conjunto de filas y producen un valor:

```sql
SELECT
    COUNT(*) AS productos,
    MIN(precio) AS precio_minimo,
    MAX(precio) AS precio_maximo,
    AVG(precio) AS precio_medio,
    SUM(stock) AS unidades
FROM producto;
```

## 2. `COUNT(*)`, `COUNT(columna)` y `COUNT(DISTINCT ...)`

- `COUNT(*)`: cuenta filas.
- `COUNT(columna)`: cuenta valores no nulos.
- `COUNT(DISTINCT columna)`: cuenta valores distintos no nulos.

```sql
SELECT
    COUNT(*) AS clientes,
    COUNT(ciudad) AS con_ciudad,
    COUNT(DISTINCT ciudad) AS ciudades_distintas
FROM cliente;
```

## 3. Efecto de `NULL`

Excepto `COUNT(*)`, los agregados ignoran normalmente los nulos. Si no hay valores no nulos, `SUM`, `AVG`, `MIN` y `MAX` devuelven `NULL`.

No sustituyas nulos por cero sin comprobar el significado:

```sql
AVG(COALESCE(nota, 0))
```

trata «sin nota» como suspenso y puede falsear el promedio.

## 4. `GROUP BY`

```sql
SELECT estado, COUNT(*) AS cantidad
FROM pedido
GROUP BY estado;
```

Produce un grupo por cada valor distinto de `estado`.

Con varias columnas:

```sql
SELECT
    strftime('%Y', fecha) AS anio,
    estado,
    COUNT(*) AS cantidad
FROM pedido
GROUP BY strftime('%Y', fecha), estado
ORDER BY anio, estado;
```

## 5. Regla de la lista `SELECT`

En una consulta agrupada, cada expresión seleccionada debe:

- aparecer en `GROUP BY`, o
- ser resultado de un agregado, o
- estar determinada funcionalmente según las capacidades del SGBD.

SQLite permite algunas consultas ambiguas que otros SGBD rechazan:

```sql
-- No portable y posiblemente ambiguo
SELECT estado, fecha, COUNT(*)
FROM pedido
GROUP BY estado;
```

¿Qué fecha representa a cada estado? Escribe consultas cuyo significado sea inequívoco.

## 6. `WHERE` frente a `HAVING`

- `WHERE` filtra filas antes de agrupar.
- `HAVING` filtra grupos después de agregar.

```sql
SELECT id_cliente, COUNT(*) AS numero_pedidos
FROM pedido
WHERE estado <> 'cancelado'
GROUP BY id_cliente
HAVING COUNT(*) >= 3
ORDER BY numero_pedidos DESC;
```

Primero elimina pedidos cancelados; después conserva clientes con al menos tres pedidos restantes.

No uses `HAVING` para filtros de filas que pueden ir en `WHERE`: expresa peor la intención y puede procesar datos innecesarios.

## 7. Agregaciones con expresiones

Importe de cada pedido:

```sql
SELECT
    id_pedido,
    SUM(cantidad * precio_unitario) AS total
FROM linea_pedido
GROUP BY id_pedido;
```

Promedio ponderado y promedio simple no son lo mismo. Define exactamente la magnitud solicitada.

## 8. Agregación condicional

```sql
SELECT
    id_cliente,
    COUNT(*) AS pedidos,
    SUM(CASE WHEN estado = 'cancelado' THEN 1 ELSE 0 END) AS cancelados
FROM pedido
GROUP BY id_cliente;
```

SQLite también admite `FILTER` en agregados en versiones actuales:

```sql
COUNT(*) FILTER (WHERE estado = 'cancelado')
```

La forma con `CASE` es más portable.

## 9. Error clásico: contar después de un `JOIN`

Si un pedido tiene varias líneas, al combinar pedidos y líneas aparecerá una fila por línea. Entonces:

```sql
COUNT(p.id_pedido)
```

cuenta líneas combinadas, no necesariamente pedidos. Puede ser necesario `COUNT(DISTINCT p.id_pedido)` o agregar antes de combinar.

## 10. Ejercicios

1. Calcula precio mínimo, máximo y medio.
2. Cuenta productos por categoría.
3. Obtén categorías con al menos cinco productos.
4. Calcula el importe total de cada pedido.
5. Cuenta pedidos no cancelados por cliente.
6. Calcula por mes el número de pedidos y el importe total.

