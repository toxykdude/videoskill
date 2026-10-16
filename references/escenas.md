# Catálogo de escenas y estilo

> Qué escena usar para cada intención y cómo debe verse y sonar el vídeo.

## Catálogo de escenas (`type` en los JSON; tiempos = segundos del corte)

### Sobre la cámara
- `callout` — rótulo grande con la cámara desenfocada; `lines`, `*acento*`, `wordsAt` (entra palabra a palabra).
  Frases clave, ganchos, conclusiones. En vertical: líneas ≤ 17 letras.
- `lowerThird` — nombre y cargo (de `marca.json`) al presentarse.
- autoLogos (`"autoLogos": true`) — ficha con logo cuando se nombra una marca de `_shared/logos/index.json`.

### A pantalla completa (fondo 3D; se adaptan a 16:9, 9:16 y 4:5)
- `orbOpen` — esfera de partículas + titular; `figure: chat|robot` (apertura de capítulo, tesis).
- `lineup` — fichas en fila (logos o texto); `strikeAt` apaga una idea (negación = gris y desenfoque, nunca tachado
  rojo); `accent` la que gana.
- `revealStat` — cifra grande + cuadrícula de 100; `display`, `label`, `fuente`, `grid:false` para no-porcentajes.
- `bars` — barras comparativas (56 % vs 28 %), `accent` en la importante, `fuente`.
- `rise` — curva que sube; `orbChips` — logos orbitando (canales); `dissolve` — palabra que se deshace;
  `outro` — cierre con kicker; `broll` — plano de apoyo a pantalla completa (la voz sigue).
- `notificacion` — notificaciones del móvil que caen (la nueva arriba empuja a las demás); `items[{app, nombre,
  titulo, texto, hora, at}]`. El problema («mientras no contestas…»).
- `buscador` — barra que se escribe sola (`query`, `typeAt`, `speed`) y despliega `results` (`accent` el bueno).
- `pasos` — ruta de 2–5 pasos (fila en 16:9, columna en vertical); `items[{title, text, logo, at}]`; el activo brilla.
- `comentario` — `estilo: "chat"` (burbujas, `yo` a la derecha, «escribiendo…») o `"comentarios"` (avatar + me gusta).

### Grueso
`zooms` (automáticos + manuales), `showcase` (recorte de pantalla en grande), `camFull` (monólogo final).

### Capas (requieren `"persona": "<id>/<parte>.persona.webm"` de `recortar.py`; solo partes a cámara, composición Parte)
- `tituloDetras` — titular gigante DETRÁS de la cabeza; `fondo: "depth"` cambia la sala por el fondo 3D (la persona
  flota). Cualquier escena a pantalla completa con `"capa": "detras"` queda detrás de la persona (presentador
  delante del gráfico), con halo de acento (`"halo": false` para quitarlo). Máx. 2–3 por vídeo; gráficos que el
  cuerpo no tape (titulares, cifras a un lado).
- `"entrada": "iris"` en una escena a pantalla completa: se abre en círculo desde la cara (transición sin corte).

### Fondos
`"fondo": "profundidad" | "puntos" | "aurora"` en el guion (los shorts lo heredan). Variarlo entre vídeos seguidos
(historial.py). Para fuentes de inspiración de fondos y movimiento: `diseno.md` §1. Ejemplo de anuncio completo:
`videos/ejemplo/anuncio.json`. Toda cifra de un estudio lleva su fuente en pantalla.

## Estilo

3D minimalista con la paleta de `marca.json`; titulares en Instrument Serif cursiva, texto Manrope, etiquetas
JetBrains Mono. Cortes secos y punch-in alterno ≤ 9 %. SFX sutiles (sin «pops»): `<Efecto nombre alinear pan>`
— `alinear` «inicio» | «pico» (el golpe del sonido cae en el fotograma) | «fin» (una subida que termina en el
revelado); por defecto el ancla de `sonidos.json`; `pan` −1…1 (lo que entra por la izquierda suena a la izquierda).
Librería de 30 sonidos (`python pipeline/sfx_generar.py`: familias golpe/aire/tick/nota/glitch, ganancia por familia).
Música: épica solo en la intro, jazz en el resto, ~16 dB bajo la voz. Logo: isotipo al abrir, logotipo tenue al
cerrar. Nunca logos encima de la pantalla del grueso. Shorts: gancho los primeros 2,8 s (en vertical grabado, a la
altura del pecho), subtítulos palabra a palabra con la activa en acento, zona segura de las apps, cierre con
`tumarca.com`.

Para decisiones visuales (composición de apertura, CTA de cierre, fichas de cristal, iconografía 3D) consultar
`diseno.md` antes de componer.
