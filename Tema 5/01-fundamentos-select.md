# Bloque 1. Fundamentos de `SELECT`

## 1. Qué es SQL

SQL —Structured Query Language— es un lenguaje declarativo para definir, consultar y manipular bases de datos relacionales. Aunque suele dividirse didácticamente en DDL, DML, DCL y TCL, estas categorías se solapan entre productos. En este tema nos concentraremos en `SELECT`.

## 2. Estructura de una consulta

```sql
SELECT expresiones
FROM origen
WHERE condicion
GROUP BY expresiones_de_agrupacion
HAVING condicion_sobre_grupos
ORDER BY expresiones
LIMIT cantidad OFFSET desplazamiento;
```

No todas las cláusulas son obligatorias. La consulta mínima en SQLite puede no tener `FROM`:

```sql
SELECT 2 + 3 AS resultado;
```

## 3. Proyección de columnas

```sql
SELECT nombre, precio
FROM producto;
```

`SELECT *` es útil para explorar, pero no suele ser apropiado en código estable:

- devuelve columnas innecesarias;
- hace menos explícito el contrato del resultado;
- puede cambiar si cambia la tabla;
- puede aumentar transferencia y procesamiento.

## 4. Expresiones y alias

La lista de selección puede contener cálculos:

```sql
SELECT
    nombre,
    precio,
    precio * 1.07 AS precio_con_impuesto
FROM producto;
```

Usa `AS` para alias legibles. Un alias no cambia el nombre almacenado y su disponibilidad en otras cláusulas depende del dialecto. Evita depender de alias fuera de `ORDER BY` si deseas consultas portables.

## 5. Eliminar duplicados

```sql
SELECT DISTINCT ciudad
FROM cliente;
```

`DISTINCT` se aplica a la combinación completa de expresiones seleccionadas:

```sql
SELECT DISTINCT ciudad, nombre
FROM cliente;
```

No garantiza una ciudad por fila si existen nombres distintos. Tampoco debe usarse para ocultar duplicados causados por un `JOIN` incorrecto.

## 6. Ordenación

```sql
SELECT nombre, precio
FROM producto
ORDER BY precio DESC, nombre ASC;
```

- `ASC` es ascendente.
- `DESC` es descendente.
- Los criterios se aplican de izquierda a derecha.

Sin `ORDER BY`, el orden no está garantizado. Para resultados reproducibles añade un último criterio único:

```sql
ORDER BY precio DESC, id_producto ASC;
```

La posición de los nulos y opciones como `NULLS FIRST`/`NULLS LAST` varían entre SGBD; SQLite admite estas opciones en su sintaxis actual.

## 7. Limitación y paginación

```sql
SELECT id_producto, nombre, precio
FROM producto
ORDER BY id_producto
LIMIT 10 OFFSET 20;
```

Esto omite 20 filas y devuelve hasta 10. Nunca pagines sin un orden estable.

Un `OFFSET` grande obliga normalmente a recorrer muchas filas. Para aplicaciones con grandes volúmenes puede ser mejor la paginación por cursor:

```sql
SELECT id_producto, nombre
FROM producto
WHERE id_producto > 200
ORDER BY id_producto
LIMIT 10;
```

## 8. Orden lógico de procesamiento

El orden escrito no coincide con el orden conceptual:

```text
FROM/JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT
```

Esto explica por qué:

- `WHERE` no puede usar normalmente una función de agregado;
- `HAVING` filtra después de agrupar;
- un alias creado en `SELECT` puede usarse en `ORDER BY`, pero no siempre en `WHERE`.

## 9. Buen formato

```sql
SELECT
    p.id_producto,
    p.nombre,
    p.precio
FROM producto AS p
WHERE p.stock > 0
ORDER BY p.precio DESC, p.id_producto;
```

Convenciones recomendadas:

- palabras reservadas en mayúsculas;
- una cláusula por línea;
- expresiones largas en líneas separadas;
- alias breves pero significativos;
- punto y coma al final.

## 10. Ejercicios

1. Lista nombre y correo de todos los clientes.
2. Obtén ciudades sin duplicados.
3. Muestra productos de mayor a menor precio, desempatando por nombre.
4. Devuelve los productos 11 a 20 con un orden estable.
5. Calcula el valor del stock como `precio * stock` y asigna un alias.

