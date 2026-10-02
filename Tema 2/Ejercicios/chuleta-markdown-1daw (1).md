# Chuleta de Markdown

## Lenguajes de Marcas — 1.º DAW

Markdown es un **lenguaje de marcas ligero** que permite estructurar documentos mediante símbolos sencillos. Sus archivos utilizan la extensión `.md` y pueden leerse incluso sin procesar.

## Encabezados

```markdown
# Título principal
## Apartado
### Subapartado
#### Nivel 4
```

- Deja un espacio después de `#`.
- Utiliza un único título principal claro.
- No saltes niveles sin necesidad.

## Párrafos y saltos

Separa los párrafos con una línea en blanco:

```markdown
Este es el primer párrafo.

Este es el segundo párrafo.
```

## Énfasis

| Resultado | Sintaxis |
|---|---|
| **Negrita** | `**Negrita**` |
| *Cursiva* | `*Cursiva*` |
| ~~Tachado~~ | `~~Tachado~~` |
| **_Negrita y cursiva_** | `**_Negrita y cursiva_**` |

## Listas

### Lista sin orden

```markdown
- HTML
- XML
- Markdown
```

### Lista numerada

```markdown
1. Crear el archivo.
2. Revisar el contenido.
3. Confirmar los cambios.
```

### Lista anidada

```markdown
- Unidad 1
  - Teoría
  - Actividades
- Unidad 2
```

## Enlaces

```markdown
[Documentación de GitHub](https://docs.github.com/)
```

Utiliza textos descriptivos. Es mejor escribir `Documentación de GitHub` que `pulsa aquí`.

## Imágenes

```markdown
![Descripción de la imagen](imagenes/captura.png)
```

El texto alternativo debe describir la imagen. Comprueba que la ruta exista también dentro del repositorio.

## Código

### Código en línea

```markdown
El archivo principal se llama `README.md`.
```

### Bloque de código

Escribe tres acentos graves antes y después. Añadir el lenguaje permite resaltar la sintaxis:

````markdown
```html
<h1>Hola, DAW</h1>
<p>Mi primer ejemplo.</p>
```
````

## Citas

```markdown
> La documentación también forma parte del proyecto.
```

## Tablas

```markdown
| Tecnología | Finalidad |
|---|---|
| HTML | Estructura web |
| CSS | Presentación |
```

- La segunda línea separa los encabezados.
- Conserva el mismo número de columnas en todas las filas.
- Puedes alinear usando `:---`, `:---:` y `---:`.

## Listas de tareas

```markdown
- [x] Repositorio creado
- [ ] Primera práctica terminada
```

- `[x]` indica una tarea completada.
- `[ ]` indica una tarea pendiente.

## Línea horizontal

```markdown
---
```

## Caracteres especiales

Antepon una barra invertida para mostrar un símbolo sin que Markdown lo interprete:

```markdown
\*Este texto muestra los asteriscos\*
```

## Ejemplo completo de README

````markdown
# Mi proyecto

Breve descripción del proyecto desarrollado en **1.º DAW**.

## Características

- Interfaz accesible.
- Diseño adaptable.
- Datos obtenidos desde una API.

## Instalación

1. Clona el repositorio.
2. Instala las dependencias.
3. Inicia la aplicación.

```bash
npm install
npm run dev
```

## Estado

- [x] Estructura inicial
- [ ] Primera versión terminada

Consulta la [documentación](https://example.com/).
````

## Errores frecuentes

- Escribir `README.txt` en lugar de `README.md`.
- No dejar un espacio después de `#`.
- No separar los párrafos con una línea en blanco.
- Olvidar cerrar un bloque de código.
- Romper una tabla al eliminar una barra vertical.
- Utilizar una ruta de imagen que solo existe en el ordenador personal.
- Usar encabezados únicamente para cambiar el tamaño del texto.
- Publicar contraseñas, tokens o datos personales.

> En GitHub, utiliza la pestaña **Preview** antes de confirmar el commit.

