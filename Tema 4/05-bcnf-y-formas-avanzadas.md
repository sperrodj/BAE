# 5. BCNF y formas normales avanzadas

## 5.1. Forma normal de Boyce-Codd (BCNF)

Una relación está en **BCNF** cuando, para toda dependencia funcional no trivial:

```text
X → Y
```

`X` es una superclave.

La BCNF es más exigente que la 3FN. En la mayoría de los ejercicios sencillos, una relación correctamente llevada a 3FN también estará en BCNF. Las diferencias aparecen especialmente cuando existen varias claves candidatas que se solapan.

### Ejemplo

Considera:

```text
ASIGNACION(estudiante, asignatura, profesor)
```

Reglas:

- cada estudiante tiene un único profesor para cada asignatura;
- cada profesor imparte una única asignatura;
- varios profesores pueden impartir la misma asignatura.

Dependencias:

```text
(estudiante, asignatura) → profesor
profesor → asignatura
```

Claves candidatas:

```text
(estudiante, asignatura)
(estudiante, profesor)
```

La relación cumple 3FN porque `asignatura` es un atributo primo: forma parte de una clave candidata. Sin embargo, no cumple BCNF porque:

```text
profesor → asignatura
```

y `profesor` no es una superclave.

### Descomposición en BCNF

```text
PROFESOR_ASIGNATURA(
  profesor PK,
  asignatura
)

ESTUDIANTE_PROFESOR(
  estudiante PK,
  profesor PK FK → PROFESOR_ASIGNATURA
)
```

> Para esta unidad, lo importante es reconocer la regla: en BCNF, todo determinante debe ser una superclave.

## 5.2. Cuarta forma normal (4FN) — ampliación

La 4FN trata dependencias multivaluadas independientes.

Supongamos que de un estudiante queremos guardar:

- las asignaturas en las que está matriculado;
- los deportes que practica;
- ambas listas son independientes.

Una tabla única produciría todas las combinaciones:

| estudiante | asignatura | deporte |
| --- | --- | --- |
| Ana | Bases de Datos | Natación |
| Ana | Bases de Datos | Baloncesto |
| Ana | Programación | Natación |
| Ana | Programación | Baloncesto |

No significa que cada deporte esté relacionado con una asignatura. La tabla mezcla dos hechos independientes.

La solución es:

```text
ESTUDIANTE_ASIGNATURA(estudiante PK, asignatura PK)

ESTUDIANTE_DEPORTE(estudiante PK, deporte PK)
```

La 4FN exige, de forma simplificada, que una relación no contenga dependencias multivaluadas independientes salvo que el determinante sea una superclave.

## 5.3. Quinta forma normal (5FN) — referencia

La 5FN estudia casos en los que una relación puede reconstruirse a partir de tres o más proyecciones y las restricciones no se explican únicamente mediante dependencias funcionales o multivaluadas.

Estos casos son poco frecuentes en aplicaciones habituales y requieren analizar dependencias de reunión. En este módulo basta con conocer su finalidad y saber que no debe descomponerse una tabla en relaciones menores sin comprobar que la unión reconstruye exactamente los datos válidos.

## 5.4. Nivel exigido

| Forma normal | Nivel de trabajo en la unidad |
| --- | --- |
| 1FN | Aplicar y justificar |
| 2FN | Aplicar y justificar |
| 3FN | Aplicar y justificar |
| BCNF | Comprender y resolver casos guiados |
| 4FN | Reconocer el problema |
| 5FN | Conocer su finalidad |

## 5.5. ¿Siempre se normaliza al máximo?

En sistemas reales puede aplicarse **desnormalización** de forma deliberada para mejorar determinadas lecturas o simplificar informes. Esta decisión solo debe tomarse después de:

- disponer de un diseño correcto;
- medir un problema real de rendimiento;
- documentar qué dato se duplica;
- establecer cómo se mantendrán sincronizadas sus copias.

La desnormalización es una decisión técnica controlada, no una excusa para evitar el análisis.
