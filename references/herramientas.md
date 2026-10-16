# Herramientas externas, miniatura y entorno

## Miniatura (Higgsfield + Remotion)

1. Fotograma nítido de la cara de esta grabación → recorte cuadrado 1024 (`out/miniatura/referencia_<id>.jpg`).
2. Higgsfield (avisar coste y saldo): `media_upload` → PUT con curl → `media_confirm`; `generate_image` con
   `params: {model: "gpt_image_2", aspect_ratio: "16:9", resolution: "2k", quality: "high", medias: [{value:
   <media_id>, role: "image"}, …], prompt}` (≈ 6,5 créditos). Boca cerrada, persona a la derecha, izquierda
   vacía y oscura, SIN texto; si hay móvil, pantalla hacia el espectador. Funciona bien: la persona señalando el
   título con un objeto relacionado desenfocado detrás.
3. Texto con Remotion: `videos/<id>/miniatura.json` {imagen, etiqueta, destacado, titulo} →
   `npx remotion still src/index.ts Miniatura ../out/miniatura/<id>_miniatura.jpg --props='{"guion":"<id>/miniatura.json"}' --image-format=jpeg --jpeg-quality=92 --scale=0.6667`.

Para la composición del titular de la miniatura (jerarquía, peso, CTA de 3–5 palabras): `diseno.md` §3 y §4.

## Herramientas externas

- **Higgsfield** (conector MCP): imágenes y vídeo (B-roll, miniaturas). NO genera música.
  - **Voz ElevenLabs v4**: `generate_audio` con `model: "elevenlabs_v4"` y `dialogue: [{text, voice_id,
    voice_type: "preset"}]` (voces de `list_voices`; todas son inglesas pero hablan español nativo: Whisper lo
    detecta «es» 0,99). Etiquetas de emoción en el texto (`[excited]`, `[curious]`, `[confident]`): no se leen.
    Coste ≈ 0,46 créditos por frase corta; 43 s de locución ≈ 4 créditos. anuncio01: voz **Andre**
    (`f1e8226e-2248-4d5f-b43c-0a79e9949dbf`, enérgica).
  - **Objetos 3D**: `gpt_image_2_5` 1:1 con 6 objetos en rejilla 3×2 sobre blanco puro y sin sombras (≈ 6,5
    créditos) → recorte local con relleno desde los bordes (scipy `ndimage.label` de lo casi blanco conectado al
    borde) → PNG con alfa por objeto. Alternativa gratuita y local sin créditos: `diseno.md` §7 (3dicons.co).
- **Música**: Biblioteca de audio de YouTube (sin atribución) → normalizada en `videos/_shared/music/`.
- **HeyGen** (conector MCP): gemelo digital, solo plan B («<look del avatar>», 25 fps, permiso previo).
- **Blotato**: no se usa; la publicación es manual.
- **Logos**: `node remotion/scripts/logos.mjs` → Simple Icons (monocromo sobre el color de la marca) + oficiales a
  color (`OFICIALES`: Lobe Icons para marcas de IA, luego SVGL, luego Lobe monocromo; si no, monograma).
  Añadir uno: `node scripts/logos.mjs oficial <slug> "<Nombre>" <#color> "<consulta>" "alias"…`.
- **Modelos locales (PyTorch cu128 en .venv)**: alineación forzada MMS_FA de torchaudio (`alinear.py`, 1,2 GB en
  la caché de torch) y Robust Video Matting (`recortar.py`, torch.hub, GPL-3.0 usado como herramienta externa).

## Entorno

- Windows 11 · i9-14900HX · RTX 4060 Laptop 8 GB · 48 GB RAM.
- Python: `.venv/` (3.13): `.venv\Scripts\python.exe pipeline\<script>.py`. ffmpeg 9 (winget Gyan.FFmpeg,
  `comun.py` lo localiza). Remotion 4 en `remotion/` (publicDir `../videos`); marca en `remotion/src/marca.ts`.
- Logos de `marca/` copiados a `videos/_shared/marca/` (Remotion solo sirve desde publicDir).
