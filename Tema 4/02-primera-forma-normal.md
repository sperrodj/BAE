# 2. Primera forma normal (1FN)

## 2.1. Regla

Una relación cumple la **primera forma normal** cuando:

- cada celda contiene un único valor del dominio correspondiente;
- no existen listas ni grupos repetitivos dentro de una columna;
- todas las filas pueden identificarse mediante una clave.

La 1FN exige valores atómicos. La atomicidad depende del uso que se hará del dato.

Por ejemplo, `nombre_completo` puede ser suficientemente atómico si solo se muestra. Si necesitamos ordenar por apellidos o dirigirnos a la persona por su nombre, conviene almacenar componentes separados.

## 2.2. Ejemplo: teléfonos

### Tabla que no cumple 1FN

| id_persona | nombre | telefonos |
| ---: | --- | --- |
| 1 | Lucía | 600111222, 922333444 |
| 2 | Omar | 611555666 |

La columna `telefonos` contiene una lista. Esto dificulta buscar, validar, actualizar o eliminar un teléfono concreto.

### Solución recomendada

#### `PERSONA`

| id_persona | nombre |
| ---: | --- |
| 1 | Lucía |
| 2 | Omar |

#### `PERSONA_TELEFONO`

| id_persona | telefono | tipo |
| ---: | --- | --- |
| 1 | 600111222 | móvil |
| 1 | 922333444 | fijo |
| 2 | 611555666 | móvil |

La clave primaria de `PERSONA_TELEFONO` puede ser `(id_persona, telefono)`. `id_persona` es además una clave ajena que referencia a `PERSONA`.

Ahora una persona puede tener cero, uno o varios teléfonos, pero cada celda contiene un solo valor.

## 2.3. Otra posible representación

También podríamos guardar una fila por teléfono en una tabla única:

| id_persona | nombre | telefono |
| ---: | --- | --- |
| 1 | Lucía | 600111222 |
| 1 | Lucía | 922333444 |
| 2 | Omar | 611555666 |

Esta tabla ya tiene valores atómicos, pero repite el nombre de la persona. Cumplir 1FN no garantiza que el diseño ya sea bueno: todavía tendremos que comprobar 2FN y 3FN.

## 2.4. Grupos repetitivos

Este diseño tampoco es adecuado:

| id_persona | nombre | telefono_1 | telefono_2 | telefono_3 |
| ---: | --- | --- | --- | --- |
| 1 | Lucía | 600111222 | 922333444 | NULL |

Problemas:

- fija artificialmente un máximo de teléfonos;
- genera valores nulos;
- obliga a repetir columnas con el mismo significado;
- complica las consultas.

La solución vuelve a ser una tabla relacionada con una fila por teléfono.

## 2.5. Cómo alcanzar 1FN

1. Localiza listas, conjuntos o columnas repetidas.
2. Decide qué representa cada valor individual.
3. Convierte cada valor en una fila independiente.
4. Crea una relación separada cuando se trate de un hecho multivaluado.
5. Define sus claves primaria y ajena.

## 2.6. Comprobación rápida

Pregúntate:

- ¿hay comas o separadores que esconden varios valores?
- ¿existen columnas como `dato1`, `dato2`, `dato3`?
- ¿puedo buscar o modificar cada valor por separado?
- ¿cada fila tiene una clave?

## 2.7. Error frecuente

Separar una dirección en otra tabla no se justifica solamente porque contenga calle, número y código postal. Eso puede ser una decisión de modelado, pero no es automáticamente una exigencia de 1FN.

La pregunta correcta es si cada columna contiene un valor adecuado para las operaciones que necesita el sistema.
