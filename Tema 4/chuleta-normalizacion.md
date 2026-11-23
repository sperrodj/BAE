# Chuleta de normalización 🧠

## Procedimiento

1. **Define la fila:** ¿qué hecho representa?
2. **Escribe las reglas:** ¿qué determina a qué?
3. **Busca las claves candidatas:** deben identificar la fila y ser mínimas.
4. **Comprueba 1FN:** un valor por celda, sin grupos repetitivos.
5. **Comprueba 2FN:** ningún atributo no clave depende de parte de una clave compuesta.
6. **Comprueba 3FN:** ningún atributo no clave depende de otro atributo no clave.
7. **Revisa BCNF:** todo determinante debe ser una superclave.
8. **Añade restricciones:** claves primarias, ajenas, `UNIQUE`, `NOT NULL` y `CHECK`.
9. **Verifica el resultado:** ¿puedes reconstruir los datos sin perder ni inventar filas?

## Resumen de formas normales

| Forma | Condición esencial | Señal de alarma |
| --- | --- | --- |
| 1FN | Un valor por celda | Listas, columnas numeradas |
| 2FN | Dependencia de la clave completa | Clave compuesta y datos que dependen de una sola parte |
| 3FN | Sin dependencias transitivas no clave | Una columna no clave describe a otra columna no clave |
| BCNF | Todo determinante es superclave | Varias claves candidatas solapadas |
| 4FN | Sin hechos multivaluados independientes | Todas las combinaciones entre dos listas |
| 5FN | Sin dependencias de reunión no justificadas | Descomposición compleja en tres o más relaciones |

## Notación básica

```text
id_cliente → nombre_cliente
```

`id_cliente` determina un único `nombre_cliente`.

```text
(num_factura, cod_producto) → cantidad
```

La pareja completa determina la cantidad.

```text
TABLA(atributo PK, atributo FK → OTRA_TABLA)
```

Indica claves primaria y ajena.

## Diagnóstico rápido

| Pregunta | Si la respuesta es «sí»… |
| --- | --- |
| ¿Una celda contiene varios valores? | Revisa 1FN |
| ¿Hay columnas `dato1`, `dato2`, `dato3`? | Revisa 1FN |
| ¿La clave es compuesta? | Busca dependencias parciales |
| ¿Un atributo no clave determina a otro? | Busca dependencia transitiva |
| ¿Un determinante no es superclave? | Revisa BCNF |
| ¿Se mezclan dos listas independientes? | Revisa 4FN |

## Errores frecuentes

- Normalizar observando solo los datos de ejemplo y no las reglas.
- Inventar dependencias que no aparecen en el enunciado.
- Creer que toda tabla con clave simple está automáticamente en 3FN.
- Eliminar la relación intermedia de una relación muchos a muchos.
- Mover un precio histórico a la tabla de productos sin analizar su significado.
- Confundir «cumple 1FN» con «ya está bien diseñada».
- Crear tablas sin indicar qué dependencia justifica cada una.
- Añadir un identificador artificial y pensar que con ello desaparecen las dependencias.

## Frase de apoyo

Los atributos no clave deben depender de:

> **la clave, toda la clave y nada más que la clave**.

Úsala como recordatorio y confirma después el resultado con las dependencias funcionales.

## Plantilla de respuesta para ejercicios

```text
1. Una fila representa: ...

2. Reglas del negocio:
   - ...

3. Claves candidatas:
   - ...

4. Dependencias funcionales:
   - X → Y

5. Forma normal inicial:
   - ... porque ...

6. Descomposición:
   - TABLA_1(...)
   - TABLA_2(...)

7. Justificación:
   - Se separa ... porque ... → ...

8. Restricciones:
   - PK: ...
   - FK: ...
   - UNIQUE/CHECK: ...
```
