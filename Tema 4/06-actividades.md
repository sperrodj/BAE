# 6. Actividades de normalización

Antes de comenzar cada actividad:

1. indica qué representa una fila;
2. escribe las reglas del negocio;
3. determina la clave o claves candidatas;
4. expresa las dependencias funcionales;
5. normaliza paso a paso y justifica cada descomposición.

## Actividad 1. Contactos y teléfonos — 1FN

```text
CONTACTO(id_contacto, nombre, telefonos)
```

| id_contacto | nombre | telefonos |
| ---: | --- | --- |
| 1 | Mónica | 600100100, 922200200 |
| 2 | Saúl | 611300300 |

Tareas:

1. Explica por qué no cumple 1FN.
2. Propón un esquema normalizado.
3. Indica las claves primarias y ajenas.
4. Añade un tipo de teléfono sin impedir que una persona tenga dos teléfonos del mismo tipo.

## Actividad 2. Matrículas — hasta 3FN

```text
MATRICULA_INICIAL(
  dni_alumno,
  nombre_alumno,
  cod_asignatura,
  nombre_asignatura,
  creditos,
  fecha_matricula
)
```

Reglas:

- un alumno puede matricularse en varias asignaturas;
- una asignatura puede tener varios alumnos;
- cada código identifica una asignatura y sus créditos;
- un alumno solo puede matricularse una vez en una asignatura durante el curso actual.

Tareas:

1. Determina la clave candidata.
2. Escribe las dependencias funcionales.
3. Identifica las dependencias parciales.
4. Descompón la relación hasta 3FN.
5. Representa el esquema con notación relacional.

## Actividad 3. Biblioteca — hasta 3FN

```text
PRESTAMO_INICIAL(
  id_prestamo,
  fecha_prestamo,
  id_socio,
  nombre_socio,
  cod_ejemplar,
  isbn,
  titulo,
  fecha_devolucion
)
```

Reglas:

- cada préstamo corresponde a un socio y a un ejemplar;
- cada ejemplar físico se identifica mediante `cod_ejemplar`;
- cada ejemplar pertenece a un libro identificado por su `isbn`;
- un ISBN determina el título;
- un préstamo puede seguir abierto y no tener aún fecha de devolución.

Tareas:

1. Escribe las dependencias funcionales.
2. Indica la forma normal máxima de la relación inicial.
3. Normaliza hasta 3FN.
4. Explica qué anomalía se produciría si cambiara el título de un libro.
5. Señala qué atributos pueden admitir `NULL` y por qué.

## Actividad 4. Pedidos — caso integrador

| pedido | fecha | cliente | nombre_cliente | producto | descripcion | cantidad | precio_aplicado |
| ---: | --- | ---: | --- | --- | --- | ---: | ---: |
| 501 | 2026-10-01 | C10 | Taller Norte | P1 | Cable USB-C | 4 | 6.50 |
| 501 | 2026-10-01 | C10 | Taller Norte | P2 | Adaptador HDMI | 1 | 18.00 |
| 502 | 2026-10-02 | C20 | Aula Digital | P1 | Cable USB-C | 10 | 5.90 |

Reglas:

- cada pedido pertenece a un cliente;
- cada producto aparece como máximo una vez en un pedido;
- el precio aplicado puede variar entre pedidos;
- el código de producto determina su descripción.

Tareas:

1. Determina la clave de la tabla.
2. Identifica las dependencias completas, parciales y transitivas.
3. Normaliza hasta 3FN.
4. Añade las claves primarias y ajenas.
5. Justifica por qué `precio_aplicado` no debe trasladarse automáticamente a `PRODUCTO`.

## Actividad 5. Detectives de errores

Un compañero propone:

```text
ALUMNO(id_alumno PK, nombre, id_curso, nombre_curso)
CURSO(id_curso PK, nombre_curso)
```

Responde:

1. ¿Qué dato está duplicado en el esquema?
2. ¿Qué dependencia funcional se está representando dos veces?
3. ¿Cómo corregirías `ALUMNO` si cada alumno solo pudiera pertenecer a un curso?
4. ¿Cómo cambiaría el diseño si cada alumno pudiera cursar varios cursos?

## Actividad 6. BCNF guiada

Considera:

```text
TUTORIA(alumno, modulo, profesor)
```

Reglas:

- cada alumno tiene un profesor para cada módulo;
- cada profesor tutoriza un único módulo;
- un módulo puede tener varios profesores.

Tareas:

1. Escribe las dependencias funcionales.
2. Obtén las claves candidatas.
3. Comprueba si la relación cumple 3FN.
4. Comprueba si cumple BCNF.
5. Si es necesario, descompón la relación.

## Actividad 7. Dos hechos independientes — 4FN

Una plataforma guarda en una misma tabla los lenguajes que conoce cada desarrollador y las certificaciones que posee. Ambos datos son independientes.

Tareas:

1. Construye datos de ejemplo que muestren el producto de combinaciones.
2. Explica por qué aparecen filas redundantes.
3. Propón dos relaciones que representen los hechos por separado.
