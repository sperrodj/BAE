# Ejercicios de diseño conceptual: modelo entidad/relación

**Bases de Datos · 1.º DAM · Tema 2**

## Instrucciones

Diseña un diagrama E/R para cada enunciado utilizando la **notación de Chen**. Trabaja únicamente con los contenidos vistos hasta ahora; no utilices elementos del modelo E/R extendido ni conviertas los diagramas en tablas.

Sigue estos pasos en cada ejercicio:

1. Marca en el enunciado las posibles entidades y relaciones.
2. Escribe frases breves que describan las reglas del problema.
3. Genera un modelo parcial para cada frase.
4. Estudia las cardinalidades: indica el mínimo y el máximo de participación de cada entidad.
5. Integra los modelos parciales en un diagrama E/R completo. Añade atributos e identificadores, incluidos los atributos que correspondan a relaciones.
6. Revisa el diagrama y anota las restricciones que no queden expresadas en él.

**Ten en cuenta:** los nombres de personajes, videojuegos y lugares sirven como ejemplos de datos. No dibujes una entidad distinta para cada personaje, mundo o juego mencionado.

## 1. Juego de Tronos: las casas de Poniente

Un archivo de Poniente recoge casas como Stark, Lannister y Targaryen. De cada casa conserva un código, su nombre, su lema y el nombre de su asentamiento principal. Una casa puede figurar en el archivo aunque todavía no se haya registrado a ninguno de sus miembros.

El archivo también recoge personajes como Arya Stark, Tyrion Lannister y Daenerys Targaryen. Cada personaje tiene un código, un nombre y una fecha de nacimiento. Su edad se obtiene a partir de esa fecha y no se guarda directamente. El título nobiliario es opcional: algunos personajes no tienen ninguno.

Para este archivo, cada personaje pertenece a una y solo una casa. Una casa puede tener cualquier número de personajes registrados.

## 2. Pokémon: la colección de capturas

Una aplicación registra entrenadores. De cada entrenador guarda un código, un nombre y una región de origen. También mantiene un catálogo de especies Pokémon: cada especie tiene su número de Pokédex, su nombre y su tipo principal.

Los entrenadores registran ejemplares concretos que han capturado. Cada ejemplar tiene un código propio, un apodo opcional, su nivel actual y la fecha de captura. Un entrenador puede tener varios ejemplares de la misma especie: por ejemplo, dos Eevee con niveles diferentes. Cada ejemplar pertenece a un único entrenador y corresponde a una única especie. Puede haber especies en el catálogo que ningún entrenador haya capturado todavía.

## 3. Breaking Bad: personajes y episodios

Una guía de *Breaking Bad* recoge personajes como Walter White, Jesse Pinkman, Saul Goodman y Gus Fring. De cada personaje guarda un código, un nombre y una ocupación. También registra los episodios mediante un código único, junto con su título, temporada, número de episodio y duración en minutos.

Un personaje puede aparecer en muchos episodios y un episodio incluye al menos un personaje registrado. La guía anota, para cada aparición concreta, el tiempo aproximado en pantalla y si el personaje tiene un papel principal o secundario en ese episodio. Un personaje recién incorporado a la guía puede no tener todavía apariciones registradas.

## 4. Minecraft: mundos y construcciones

Un servidor de *Minecraft* permite crear mundos como «Supervivencia» y «Creativo». De cada mundo guarda un código, un nombre y su fecha de creación. Un mundo puede existir aunque todavía no tenga construcciones.

En cada mundo se registran construcciones, como un castillo, una granja o una estación. El número de construcción solo es único dentro de su mundo: puede existir la construcción número 12 tanto en «Supervivencia» como en «Creativo». De cada construcción se guarda su número, nombre y fecha de inicio. Una construcción no puede existir sin su mundo.

El servidor registra además jugadores mediante un código y un nombre de usuario. Varios jugadores pueden colaborar en una construcción, y un jugador puede colaborar en varias construcciones. De cada colaboración se guarda la fecha en que comenzó. Se permite registrar construcciones en las que aún no haya colaboradores.

## 5. Fortnite: escuadrones y partidas

Una aplicación de *Fortnite* registra jugadores mediante un código, un nombre de usuario y su fecha de alta. Un jugador puede pertenecer a un escuadrón como máximo, y un escuadrón puede tener varios jugadores. También se permite crear un escuadrón antes de incorporar jugadores. De cada escuadrón se guarda un código y un nombre.

La aplicación registra partidas mediante un código, fecha, hora y modo de juego. **Cada partida registrada tiene al menos dos jugadores.** Un jugador puede participar en muchas partidas. De la participación de cada jugador en cada partida se almacena su posición final y el número de eliminaciones. Un jugador participa una sola vez en una misma partida. Es posible que un jugador recién registrado todavía no haya disputado ninguna.

Para simplificar, no se registra con qué escuadrón jugó cada participante en cada partida.

## 6. Arcane: personajes y vínculos familiares

Una enciclopedia de *Arcane* registra personajes como Vi, Jinx, Caitlyn, Ekko y Vander. De cada personaje guarda un código, su ciudad de procedencia y su nombre completo. El nombre completo se divide, cuando se conoce, en nombre y apellido; el apellido puede no constar.

La enciclopedia quiere registrar qué personajes son hermanos entre sí. Un personaje puede no tener hermanos registrados o tener varios. Si se indica que Vi es hermana de Jinx, debe entenderse también que Jinx es hermana de Vi. Ningún personaje puede ser hermano de sí mismo.

También registra lugares que aparecen en la historia, como Piltover, Zaun y el Puente del Progreso. De cada lugar se guarda un código y un nombre. Un personaje puede visitar varios lugares y un lugar puede ser visitado por varios personajes. De cada visita registrada se conoce la fecha dentro de la cronología de la enciclopedia. **Se registra como máximo una visita por cada pareja personaje-lugar.**

## 7. Elden Ring: zonas, jefes y victorias

Una guía de *Elden Ring* registra zonas como Necrolimbo, Liurnia y Caelid. De cada zona guarda un código, su nombre y un nivel recomendado. También recoge jefes como Margit, Rennala y Radahn. Cada jefe tiene un código, un nombre y la cantidad de runas que concede al derrotarlo por primera vez.

Cada jefe aparece en una sola zona de esta guía. Una zona puede tener varios jefes o ninguno.

Los jugadores registrados tienen un código y un alias. Un jugador puede derrotar a muchos jefes, y cada jefe puede ser derrotado por muchos jugadores. La guía registra una única victoria por cada pareja jugador-jefe, junto con la fecha de esa primera victoria y el número de intentos necesarios. Un jugador nuevo puede no haber derrotado a ninguno.

## 8. Valorant: jugador, agente y partida

Una aplicación de estadísticas de *Valorant* registra jugadores mediante un código y un nombre de usuario; agentes mediante un código, un nombre y un rol; y partidas mediante un código, una fecha y un mapa.

Quiere almacenar qué agente utilizó cada jugador en cada partida. Un jugador puede disputar muchas partidas y utilizar distintos agentes en ellas. Un agente puede ser utilizado por muchos jugadores y aparecer en muchas partidas. Cada jugador que participa en una partida utiliza exactamente un agente en ella.

Para cada combinación registrada de jugador, agente y partida se guardan las eliminaciones conseguidas. Una partida debe contar con al menos un jugador, pero se permite registrar jugadores y agentes que todavía no hayan intervenido en ninguna partida.

No es necesario representar equipos ni rondas.

## 9. Nintendo: versiones de juegos

Un catálogo registra juegos como *The Legend of Zelda: Breath of the Wild*, *Mario Kart 8 Deluxe* y *Animal Crossing: New Horizons*. De cada juego guarda un código, su título y su género. También registra consolas, como Nintendo Switch, Nintendo Switch 2 y Nintendo 3DS, con un código, un nombre y su año de lanzamiento.

Un juego puede tener versiones para varias consolas, y una consola puede disponer de muchos juegos. Para cada versión de un juego en una consola se conserva su fecha de lanzamiento y su precio inicial. Un juego anunciado puede estar registrado antes de que se haya publicado ninguna versión. **En este catálogo existe como máximo una versión para cada pareja juego-consola.**

El catálogo recibe reseñas de usuarios. Cada usuario tiene un código y un alias. Una reseña se refiere a una versión concreta de un juego, no al juego en general: una persona podría valorar de forma diferente sus versiones para dos consolas. De cada reseña se guarda un código propio, una puntuación del 1 al 10 y un comentario opcional. Cada reseña pertenece a un único usuario. Un usuario puede escribir varias reseñas, pero como máximo una por cada versión.
