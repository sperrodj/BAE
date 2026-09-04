# Bloque 2. Filtrado, expresiones y funciones

## 1. `WHERE`

`WHERE` conserva las filas cuya condición resulta verdadera.

```sql
SELECT nombre, precio
FROM producto
WHERE precio >= 20;
```

## 2. Comparaciones

```text
=   <>   <   <=   >   >=
```

Algunos productos aceptan `!=`, pero `<>` es el operador SQL estándar para «distinto».

```sql
SELECT *
FROM pedido
WHERE estado <> 'cancelado';
```

## 3. Lógica y precedencia

```sql
SELECT nombre, precio, stock
FROM producto
WHERE (precio < 10 OR precio > 100)
  AND stock > 0;
```

`NOT` se evalúa antes que `AND`, y `AND` antes que `OR`. Usa paréntesis cuando una lectura equivocada sea posible.

## 4. Rangos y listas

`BETWEEN` incluye ambos extremos:

```sql
WHERE precio BETWEEN 10 AND 30
```

equivale a:

```sql
WHERE precio >= 10 AND precio <= 30
```

`IN` compara con una lista:

```sql
WHERE estado IN ('pendiente', 'pagado', 'enviado')
```

Para intervalos temporales suele ser más seguro usar un rango semiabierto:

```sql
WHERE fecha >= '2026-09-01'
  AND fecha <  '2026-10-01'
```

Así se incluyen todas las horas de septiembre si la columna contiene fecha y hora.

## 5. `NULL` y lógica de tres valores

Las comparaciones con `NULL` producen desconocido:

```sql
-- Incorrecto
WHERE ciudad = NULL

-- Correcto
WHERE ciudad IS NULL
```

También existen `IS NOT NULL`. Ten cuidado con:

```sql
WHERE ciudad <> 'Arrecife'
```

No incluye filas con ciudad nula. Si deben incluirse:

```sql
WHERE ciudad <> 'Arrecife' OR ciudad IS NULL
```

### `NOT IN` y nulos

Si la lista o subconsulta contiene `NULL`, `NOT IN` puede no devolver lo esperado. Para antijoin suele ser más robusto `NOT EXISTS`.

## 6. Patrones sencillos

```sql
SELECT nombre
FROM producto
WHERE nombre LIKE 'Teclado%';
```

- `%`: cero o más caracteres.
- `_`: exactamente un carácter.

Para buscar un comodín literal:

```sql
WHERE codigo LIKE '%!%%' ESCAPE '!'
```

El tratamiento de mayúsculas, minúsculas y Unicode depende del SGBD y la intercalación. En SQLite, `LIKE` ignora diferencias de caso para ASCII por defecto, pero no realiza plegado completo de Unicode sin extensiones.

Los patrones avanzados se desarrollan en el bloque 7.

## 7. Expresiones condicionales

```sql
SELECT
    nombre,
    stock,
    CASE
        WHEN stock = 0 THEN 'agotado'
        WHEN stock < 5 THEN 'bajo'
        ELSE 'disponible'
    END AS situacion
FROM producto;
```

`CASE` devuelve un valor; no ejecuta flujo de control del programa.

## 8. Funciones de nulos

```sql
SELECT nombre, COALESCE(ciudad, 'Sin informar') AS ciudad
FROM cliente;
```

`COALESCE` devuelve el primer argumento no nulo.

```sql
SELECT NULLIF(stock, 0)
FROM producto;
```

`NULLIF(a,b)` devuelve `NULL` si ambos son iguales; en caso contrario devuelve `a`.

## 9. Funciones habituales de SQLite

### Texto

```sql
SELECT
    upper(nombre),
    lower(nombre),
    length(nombre),
    trim(nombre),
    substr(nombre, 1, 10)
FROM producto;
```

Concatenación en SQLite y SQL estándar:

```sql
SELECT nombre || ' <' || email || '>' AS contacto
FROM cliente;
```

### Números

```sql
SELECT round(precio, 2), abs(stock)
FROM producto;
```

### Tipos

```sql
SELECT CAST(precio AS TEXT)
FROM producto;
```

No confundas conversión con validación del dato original.

### Fechas en SQLite

SQLite no tiene un tipo fecha/hora dedicado con el mismo comportamiento que otros SGBD; proporciona funciones sobre representaciones compatibles.

```sql
SELECT date('now');
SELECT datetime('now');
SELECT strftime('%Y-%m', fecha) AS mes
FROM pedido;
```

Almacenar fechas en formato ISO 8601 facilita ordenar y comparar texto cuando el formato es coherente.

## 10. Ejercicios

1. Productos con precio entre 10 y 50 y stock positivo.
2. Pedidos cuyo estado esté entre tres valores permitidos.
3. Clientes sin ciudad registrada.
4. Productos cuyo nombre comience por `A`.
5. Etiqueta cada producto como agotado, bajo o disponible.
6. Muestra el mes de cada pedido en formato `AAAA-MM`.

