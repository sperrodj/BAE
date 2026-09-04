# Bloque 7. Patrones y expresiones regulares

## 1. Elegir la herramienta adecuada

| Necesidad | Herramienta |
|---|---|
| Igualdad exacta | `=` |
| Prefijo, sufijo o fragmento sencillo | `LIKE` |
| Patrón tipo nombres de archivo en SQLite | `GLOB` |
| Patrón complejo | Expresión regular, si está habilitada |
| Búsqueda lingüística o por relevancia | Motor de texto completo |

No uses expresiones regulares cuando una igualdad o un rango expresa mejor la intención.

## 2. `LIKE`

```sql
SELECT nombre
FROM producto
WHERE nombre LIKE 'Monitor%';
```

- `%`: cualquier secuencia, incluso vacía.
- `_`: exactamente un carácter.

```sql
-- Código formado por A, un carácter cualquiera y 20 al final
WHERE codigo LIKE 'A_20'
```

### Escape

```sql
-- Busca valores que contienen un porcentaje literal
WHERE descripcion LIKE '%!%%' ESCAPE '!'
```

## 3. Sensibilidad a mayúsculas y Unicode

En SQLite, `LIKE` ignora diferencias de mayúsculas para caracteres ASCII por defecto, pero no realiza la misma equivalencia para todo Unicode. La intercalación y extensiones como ICU pueden cambiar el comportamiento.

No uses transformaciones improvisadas como garantía universal de comparación lingüística. Define qué significa igualdad para el dominio y configura el SGBD apropiadamente.

## 4. `GLOB` en SQLite

`GLOB` utiliza comodines de estilo Unix y distingue mayúsculas:

- `*`: cualquier secuencia;
- `?`: un carácter;
- `[abc]`: un carácter del conjunto;
- `[a-z]`: un carácter del rango;
- `[^0-9]`: un carácter fuera del conjunto.

```sql
SELECT nombre
FROM producto
WHERE nombre GLOB '[A-C]*';
```

`GLOB` no usa `%` ni `_` y no dispone de cláusula `ESCAPE` como `LIKE`.

## 5. `REGEXP` en SQLite

SQLite reconoce sintácticamente:

```sql
expresion REGEXP patron
```

pero la función `regexp()` no está implementada en la distribución básica. La aplicación o una extensión debe registrarla. Por tanto, esta consulta puede fallar en un entorno donde no se haya habilitado:

```sql
SELECT email
FROM cliente
WHERE email REGEXP '^[^@]+@[^@]+\.[^@]+$';
```

La sintaxis exacta del patrón depende del motor de expresiones regulares proporcionado.

> [!WARNING]
> Una expresión regular sencilla no demuestra que una dirección de correo exista ni que pueda recibir mensajes. La validación completa incluye reglas de aplicación y, cuando proceda, verificación.

## 6. Elementos habituales de regex

Cuando el motor los soporte:

| Patrón | Significado general |
|---|---|
| `^` | inicio del texto |
| `$` | final del texto |
| `.` | cualquier carácter |
| `*` | cero o más repeticiones |
| `+` | una o más repeticiones |
| `?` | cero o una repetición |
| `[A-Z]` | carácter dentro de un rango |
| `[^0-9]` | carácter fuera de un conjunto |
| `{2,4}` | entre dos y cuatro repeticiones |
| `(ab|cd)` | una alternativa u otra |

Comprueba siempre el dialecto del motor: escapes, Unicode, clases y operadores pueden variar.

## 7. Rendimiento

Los patrones con prefijo fijo pueden aprovechar ciertos índices según configuración y colación:

```sql
WHERE nombre LIKE 'Monitor%'
```

Los comodines iniciales suelen impedir una búsqueda directa mediante un índice B-tree ordinario:

```sql
WHERE nombre LIKE '%monitor%'
```

Para búsqueda de texto a escala, considera las capacidades de texto completo del SGBD en vez de acumular regex complejas.

## 8. Seguridad

Si un patrón procede del usuario:

- usa parámetros, igual que con otros valores SQL;
- limita longitud y complejidad;
- contempla patrones de coste excesivo según el motor;
- no construyas SQL concatenando texto recibido.

```sql
-- La aplicación enlaza :patron como parámetro
SELECT nombre
FROM producto
WHERE nombre LIKE :patron;
```

## 9. Ejercicios

1. Busca productos que empiezan por `Teclado` con `LIKE`.
2. Busca un guion bajo literal mediante `ESCAPE`.
3. En SQLite, encuentra nombres que empiezan por una letra entre A y C con `GLOB`.
4. Explica por qué `REGEXP` puede no funcionar en una instalación básica.
5. Compara el coste probable de `texto%` y `%texto%`.
6. Diseña pruebas para un patrón de correo sin afirmar que valida la existencia de la dirección.

## Referencias

- [SQLite: operadores LIKE, GLOB y REGEXP](https://www.sqlite.org/lang_expr.html#the_like_glob_regexp_match_and_extract_operators)

