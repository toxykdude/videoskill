# Flujo completo de un proyecto

> Manual operativo paso a paso. El resumen vive en `../SKILL.md`; aquí está el detalle con comandos.

## 1. Entregables de CADA proyecto

1. **YouTube horizontal** (1920×1080, −14 LUFS): partes → montaje con capítulos, descripción y miniatura.
2. **UN vertical grabado aparte** (móvil en vertical) → 3 versiones con textos distintos: **YouTube Shorts con
   música**, **Instagram Reels SIN música** y **TikTok SIN música** (el usuario pone la música al publicar).
3. **LinkedIn**: vídeo 4:5 (el mismo vertical, sin música), carrusel PDF, imagen suelta y texto del post.
4. Si el usuario no trae guiones, Claude los escribe (`videos/_shared/plantillas/guiones.md`) y le entrega una guía de
   grabación paso a paso (ejemplo real: `videos/proyecto01/guia_grabacion.md`).

Todo termina en `<DRIVE>/final/<id>/` (`shorts/`, `linkedin/`). La publicación es manual.

## 2. Estructura

```
marca/            identidad (único sitio con nombres, colores, logos, redes)
pipeline/         scripts Python (numerados en el orden del flujo) + comun.py
remotion/         composiciones: Intro|Disclaimer|Catalogo (Parte), Grueso/GruesoGraficos, Short, Linkedin,
                  Lamina, Miniatura · escenas en src/escenas · scripts/qa.mjs, laminas.mjs, logos.mjs
videos/           publicDir de Remotion: SOLO lo que Remotion lee (cortes finales, JSON, broll, música, logos)
  _shared/        music/, logos/, fx/, marca/, descripciones/plantilla.txt, plantillas/guiones.md
  <id>/           plan.md, guia_grabacion.md, partes.json, <parte>.json, shorts.json, linkedin.json,
                  capitulos_youtube.txt, resumen.txt, miniatura*.json, broll/
raw/<id>/         brutos copiados de Drive + intermedios pesados (partes/*_bruto.mov, grueso_*.mp4)
out/              renders por parte, qa/, miniatura/, entregas/<id>/ (lo que va a Drive), lote_<id>.cmd/.log
```

## 3. Flujo paso a paso (como proyecto01)

**0. Preparar** — guion y guía de grabación (`guia_grabacion.md`); si hay demo de la app, que sea un tenant con
datos ficticios (así no hay que pixelar). Grabación: móvil horizontal a la altura de los ojos (cámara frontal +
Nano Teleprompter), audio con el micrófono USB en OBS (plan B probado) o directo al móvil; claqueta «Bloque X, toma Y» y
palmada; bloque de pantalla con OBS + móvil; vertical aparte de pie.

**1. Traer y revisar** — `python pipeline/brutos.py traer <id> …` (o copia con `comun.copiar_reanudable`).
`ffprobe` de todo: rotación, resolución, fps. Mirar un fotograma con texto: **la cámara frontal graba en espejo**.

**2. Partes de cámara** (intro / datos / cierre…; una sesión con todos los bloques seguidos):
```
python pipeline/05_sync.py <obs_audio.mp4> <movil.mp4> videos/<id>/camara.sync.json      # desfase por voz
#  sesión = vídeo del móvil (hflip, fps=30) + audio de OBS desplazado → raw/<id>/camara_sesion.mov (ffmpeg)
python pipeline/02_transcribe.py raw/<id>/camara_sesion.mov videos/<id>/sesion.transcripcion.json
#  tramos de cada parte (tras la claqueta, ±0,3 s) → videos/<id>/partes.json
python pipeline/extraer_partes.py raw/<id>/camara_sesion.mov videos/<id>/partes.json  # → <parte>_bruto.mov
#  (mover los *_bruto.mov a raw/<id>/partes/: publicDir ligero)
python pipeline/02_transcribe.py raw/<id>/partes/<parte>_bruto.mov videos/<id>/<parte>.transcripcion.json
python pipeline/03_tomas.py videos/<id>/<parte>.transcripcion.json videos/<id>/<parte>.palabras.json [--quitar i-j]
python pipeline/01_cut_silences.py raw/<id>/partes/<parte>_bruto.mov videos/<id>/<parte>.mp4 --words videos/<id>/<parte>.palabras.json
#  (01_cut_silences y 04_grueso llaman solos a alinear.py: tiempos de palabra exactos; --sin-alinear para no)
python pipeline/recortar.py videos/<id>/<parte>.mp4 [--desde a --hasta b]   # opcional: persona con alfa (capas)
```
Leer el «Texto final» de 03_tomas: quitar a mano deslices que cambian el sentido. Verificación ≥ 98 %.

**3. Concepto y escenas**
```
python pipeline/lectura.py <id> <parte>          # ritmo, énfasis, cifras, marcas, bloques → <parte>.lectura.md
python pipeline/historial.py comparar <id>       # qué repetiría respecto a los últimos vídeos; fondo sugerido
#  → videos/<id>/concepto.md (plantilla videos/_shared/plantillas/concepto.md): idea, fondo, piezas, capas, música
#  → videos/<id>/<parte>.json (catálogo en escenas.md) anclado a <parte>.palabras_corte.json; "ancla" = frase que se oye
python pipeline/golpe_musica.py videos/<id>/intro.json --en <seg del momento clave> [--tambien …] --aplicar 1
```
Música: intro épica 0,16; resto jazz 0,13; `musica.desde` = segundo del tema en que arranca (golpe_musica.py lo
cuadra para que un golpe caiga en el momento clave). `camara.foco` = posición de la cara (medir con rejilla).
QA: `node remotion/scripts/qa.mjs Intro <id>/<parte>.json 3 9 17 … --nombre x` y MIRAR los fotogramas.
Tras el render de cada parte: `python pipeline/verificar_sync.py videos/<id>/<parte>.json out/<id>_<parte>.mp4`
→ duraciones, voz vs corte y cada escena contra su ancla + hoja de contactos `out/qa/<id>_<parte>_sync.jpg`.

Para la fase de diseño de este paso, consultar `diseno.md` (inspiración por intención de bloque).

**4. Bloque de pantalla (grueso)**
```
python pipeline/05_sync.py <pantalla.mp4> <movil.mp4> videos/<id>/grueso.sync.json
python pipeline/02_transcribe.py <pantalla.mp4> videos/<id>/grueso.transcripcion.json
python pipeline/03_tomas.py videos/<id>/grueso.transcripcion.json videos/<id>/grueso.palabras.json
#  grueso.prep.json: recorte/rellenar, voltearCamara, quitar (arranques y «esto no sale»), pixelar
#  (datos reales: hojas.py + pixelar_auto.py; datos ficticios: solo la cuenta de la cabecera)
python pipeline/04_grueso.py videos/<id>/grueso.prep.json
python pipeline/06_zooms.py videos/<id>/grueso_pantalla.mp4 videos/<id>/grueso.json
#  + acercamientos manuales a lo que se nombra (frases del corte ↔ posición en pantalla)
#  mover grueso_pantalla/camara a raw/<id>/partes/ y apuntar grueso.json a ../raw/...
python pipeline/render_grueso.py videos/<id>/grueso.json        # híbrido ffmpeg + Remotion, ~3–10 min
```

**5. B-roll, miniatura y renders**
- B-roll: Higgsfield Kling 3.0 std 5 s sin sonido (~6,25 créditos/clip) → `videos/<id>/broll/` (1080p30, `-g 15`).
- Miniatura: ver `herramientas.md`. Dos variantes, elegir la que no choca con el texto.
- Renders de las partes de cámara: proceso independiente `out/lote_<id>.cmd` (Start-Process), ~20–25 min por
  3 min de vídeo (`npx remotion render src/index.ts Intro ../out/<id>_<parte>.mp4 --props=… --timeout=180000`).

**6. Montaje final** — `capitulos_youtube.txt` (tiempos relativos a cada parte) + `resumen.txt` (con fuentes):
```
python pipeline/07_final.py out/entregas/<id> "<Título>.mp4" out/<id>_intro.mp4 out/<id>_datos.mp4 out/<id>_grueso.mp4 out/<id>_cierre.mp4 --id <id> --capitulos videos/<id>/capitulos_youtube.txt --titulo "<Título>" --miniatura out/miniatura/<id>_miniatura.jpg
```
→ −14 LUFS, capítulos válidos, descripción, miniatura, y copia a Drive. Revisar las uniones entre partes.

**7. Vertical + LinkedIn**
```
#  vertical: 02 → 03 → 01 sobre el bruto (hflip; sale 1080×1920) · videos/<id>/vertical.json con escenas
#  shorts.json: {"parte": "vertical", "tramos": [[0, dur]], "gancho": [...], "textos": {youtube, instagram, tiktok, linkedin}}
python pipeline/09_shorts.py <id> --sin-render ; node remotion/scripts/qa.mjs Short <id>/shorts/<n>/short.json 1.2 7 22
python pipeline/09_shorts.py <id>          # YT (música) + IG + TikTok + LinkedIn 4:5 + portada + textos → Drive
python pipeline/10_linkedin.py <id>        # carrusel PDF + JPG + imagen + post → Drive
```
Anuncio corto (ejemplo real: `videos/promo01/`, promo de 12 s grabada por otra persona): mismo flujo de vertical,
escenas densas (gancho → `notificacion` del problema → marca a cámara con autoLogo → `comentario` chat que
responde → `revealStat` cifra sola) y `"cola": 4` en el short = segundos extra tras la última palabra (cámara
congelada, sin voz) para un `outro` de marca con `tumarca.com`; `"cierre": ""` quita el rótulo pequeño.

**Anuncio de producto animado con locución** (ejemplo real: `videos/anuncio01/`, réplica del estilo de un anuncio
de SaaS que el usuario grabó de Instagram): sin cámara; la app de la marca recreada en paneles 3D.
```
#  1. Estudiar la referencia: hojas de 1 fps (ffmpeg tile) + transcribir su locución → estructura de frases.
#  2. Pantallas reales de la app (p. ej. raw/<id>/partes/grueso_pantalla.mp4 de proyecto01) → datos de demostración.
#  3. Guion de locución (≈ 40–45 s, una idea por frase) → Higgsfield `elevenlabs_v4` (ver herramientas.md) → voz.
#  4. videos/<id>/locucion.voz.wav (−16 LUFS) + palabras (Whisper) + alinear.py → tiempos de cada palabra.
#  5. videos/<id>/anuncio.json: escenas {tipo, from, to, transicion, at:{…palabra…}} (tiempos sacados de las
#     palabras con un script) + musica {src, volumen, desde} + portada + textos por red.
node remotion/scripts/qa.mjs Anuncio <id>/anuncio.json 1.2 7.9 … ; … AnuncioLinkedin …   # ¡mirar 9:16 y 4:5!
python pipeline/anuncio.py <id>     # render Anuncio + AnuncioLinkedin → YT (música), IG, TikTok, LinkedIn → Drive
```
Escenas (`remotion/src/promo/`): `apertura` (mancha orgánica + titular cinético), `canales` (logo + canales que
se juntan en una bandeja), `medianoche` (teléfono 3D con notificaciones), `chat` (vendedor IA escribiendo + link
de pago), `leads` (tabla con puntaje y resaltado caliente/tibio/frío sincronizado), `handoff` (traspaso con
contexto), `pago` (comprobante + cursor que pulsa), `listo` (check con estallido), `pildoras` (columnas de
píldoras + frase), `hoy` (panel de tareas del día), `recupera` (cursor + barra + pago recuperado), `remate`
(frases cinéticas + cifra gigante), `final` (logo, lema y botón). `"transicion": "mancha"` = barrido orgánico con agujero.
Mundo claro derivado de `brillo` + paneles oscuros de la app + objetos 3D (`videos/_shared/obj/`; ver `diseno.md` §7
para objetos 3D gratuitos).

**8. Cerrar** — `python pipeline/historial.py registrar <id>`; informe para el usuario (`out/entregas/<id>/LEEME_<id>.md`,
también en Drive): qué hay, qué revisar, créditos gastados, textos listos para pegar por red.
