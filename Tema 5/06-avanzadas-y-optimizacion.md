# Bloque 6. Consultas avanzadas y optimización

## 1. Expresiones de tabla comunes — CTE

Una CTE asigna un nombre temporal a una consulta:

```sql
WITH totales AS (
    SELECT
        p.id_cliente,
        SUM(lp.cantidad * lp.precio_unitario) AS total
    FROM pedido AS p
    JOIN linea_pedido AS lp
      ON lp.id_pedido = p.id_pedido
    GROUP BY p.id_cliente
)
SELECT c.nombre, t.total
FROM totales AS t
JOIN cliente AS c
  ON c.id_cliente = t.id_cliente
WHERE t.total > 500
ORDER BY t.total DESC;
```

Mejora la estructura de consultas complejas. No garantiza por sí sola mejor rendimiento: el optimizador decide cómo tratarla.

## 2. CTE recursiva

Permite recorrer jerarquías:

```sql
WITH RECURSIVE arbol(id_categoria, nombre, nivel) AS (
    SELECT id_categoria, nombre, 0
    FROM categoria
    WHERE id_padre IS NULL

    UNION ALL

    SELECT c.id_categoria, c.nombre, a.nivel + 1
    FROM categoria AS c
    JOIN arbol AS a
      ON c.id_padre = a.id_categoria
)
SELECT *
FROM arbol
ORDER BY nivel, nombre;
```

Consta de un caso base y un paso recursivo. Deben prevenirse ciclos o profundidad descontrolada cuando el dominio pueda contenerlos.

## 3. Funciones de ventana

Calculan valores sobre conjuntos relacionados sin reducir todas las filas a un grupo.

```sql
SELECT
    id_producto,
    nombre,
    id_categoria,
    precio,
    AVG(precio) OVER (
        PARTITION BY id_categoria
    ) AS media_categoria
FROM producto;
```

### Ranking

```sql
SELECT
    nombre,
    id_categoria,
    precio,
    ROW_NUMBER() OVER (
        PARTITION BY id_categoria
        ORDER BY precio DESC, id_producto
    ) AS posicion
FROM producto;
```

- `ROW_NUMBER`: numeración sin empates.
- `RANK`: deja huecos después de empates.
- `DENSE_RANK`: no deja huecos.

### Acumulados

```sql
SELECT
    fecha,
    id_pedido,
    SUM(total) OVER (
        ORDER BY fecha, id_pedido
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS acumulado
FROM resumen_pedido;
```

Especificar el marco evita resultados sorprendentes con empates.

## 4. Corrección antes que velocidad

Optimizar una consulta incorrecta solo produce antes el resultado equivocado. Primero:

- define la cardinalidad esperada;
- prueba nulos, duplicados y ausencia de coincidencias;
- compara con un resultado pequeño calculado manualmente.

## 5. Seleccionar solo lo necesario

Evita `SELECT *` y filtros tardíos. Reduce columnas y filas tan pronto como sea coherente con la consulta.

## 6. Índices y condiciones aprovechables

Los índices pueden ayudar en columnas usadas para:

- igualdad y rangos en `WHERE`;
- condiciones de combinación;
- ordenación;
- agrupamiento;
- restricciones de unicidad.

No todos los índices ayudan. Ocupan espacio y encarecen escrituras.

Una condición suele ser más aprovechable si no transforma la columna indexada:

```sql
-- Menos favorable para un índice ordinario sobre fecha
WHERE strftime('%Y', fecha) = '2026'

-- Rango directo
WHERE fecha >= '2026-01-01'
  AND fecha <  '2027-01-01'
```

SQLite permite índices sobre expresiones en casos concretos, pero la expresión debe coincidir con la consulta.

## 7. Índices compuestos

El orden importa. Un índice `(id_cliente, fecha)` suele ayudar a buscar por cliente y después por fecha, pero no necesariamente a buscar solo por fecha.

Diseña índices a partir de consultas reales, selectividad y ordenaciones, no añadiendo uno a cada columna.

## 8. Plan de ejecución

En SQLite:

```sql
EXPLAIN QUERY PLAN
SELECT *
FROM pedido
WHERE id_cliente = 10
ORDER BY fecha DESC;
```

El plan puede mostrar exploraciones completas, búsquedas por índice, ordenaciones temporales y orden de combinaciones.

No memorices únicamente palabras como `SCAN` o `SEARCH`: interpreta el plan junto con el tamaño de las tablas y la consulta.

## 9. Estadísticas

El optimizador estima costes mediante estadísticas. En SQLite, `ANALYZE` recopila información útil para elegir planes. Un buen índice puede no elegirse si las estadísticas no reflejan los datos actuales.

## 10. Antipatrones

- `DISTINCT` para esconder un `JOIN` incorrecto.
- `ORDER BY` dentro de una subconsulta suponiendo que el orden se conservará fuera.
- funciones sobre columnas filtradas sin estudiar índices.
- comodines iniciales como `LIKE '%texto'` esperando una búsqueda indexada ordinaria.
- subconsultas repetidas que podrían calcularse una vez.
- paginación con grandes `OFFSET`.
- índices redundantes o sin uso.
- optimizar sin medir.

## 11. Proceso de optimización

1. Define resultado y tiempo objetivo.
2. Usa datos representativos.
3. Mide una línea base.
4. Obtén el plan.
5. Localiza el coste dominante.
6. Cambia una sola cosa.
7. Verifica corrección y vuelve a medir.
8. Documenta el resultado.

## 12. Ejercicios

1. Reescribe una consulta larga mediante CTE.
2. Obtén los tres productos más caros por categoría.
3. Calcula un acumulado mensual.
4. Compara un filtro de año mediante función y mediante rango.
5. Propón un índice para pedidos por cliente y fecha; justifica el orden.
6. Analiza dos planes con `EXPLAIN QUERY PLAN`.

