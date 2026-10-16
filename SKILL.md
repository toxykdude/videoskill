---
name: videoskill
description: "Trigger: editar vídeo, brutos, montaje, shorts, reels, tiktok, linkedin, miniatura, anuncio, Remotion. Pipeline de vídeo con IA: corta silencios por energía de voz, sincroniza multicámara, anima escenas 3D y entrega cada red."
license: MIT
metadata:
  author: toxykdude
  version: "2.0.0"
---

# Videoskill — edición de vídeo de punta a punta (marca blanca)

Skill marca blanca: nada de nombres, logos ni colores fijos en el código. Todo sale de
`marca/` (`marca.json` identidad y colores, `canal.json` canal y redes, `proyecto.json`
equipo y carpetas). Idioma de trabajo: español.

## Activation Contract

Úsalo cuando el usuario pida editar un vídeo o sus brutos, sacar shorts / reels / TikTok,
montar un episodio de YouTube, crear un anuncio animado con locución, una miniatura o el
carrusel de LinkedIn. También cuando traiga guiones para grabar (`videos/_shared/plantillas/guiones.md`)
o quiera ejecutar el flujo completo de un proyecto (`proyecto01`, `promo01`, `anuncio01`).

## Reglas duras

1. **No publicar nada, no gastar créditos ni subir archivos a ningún servicio sin preguntar**
   (excepción SIEMPRE: copiar las entregas a su Drive `final/<id>/`). Al gastar créditos,
   decir cuánto y cuánto queda.
2. Enlaces públicos: SIEMPRE `https://tumarca.com` (nunca el dominio largo). Redes en `marca/canal.json`.
3. Cámara real siempre; un gemelo digital (p. ej. HeyGen) solo como plan B.
4. **Antes de tocar nada, leer `references/reglas-tecnicas.md`.** Cuando algo falle y se
   arregle, apuntarlo allí: el sistema aprende con cada vídeo.
5. Antes de diseñar escenas, fondos, fichas, miniaturas u outros, consultar `references/diseno.md`
   (fuentes de inspiración verificadas y mapeadas al pipeline).
6. `npx tsc --noEmit` antes de cada render; QA de fotogramas SIEMPRE
   (`node remotion/scripts/qa.mjs …`) y MIRARLOS antes del render completo.
7. Procesar siempre desde disco local; en `<DRIVE>` solo se COPIA, nunca se procesa
   (`pipeline/brutos.py traer|entregar <id>`).

## Estructura

```
marca/            identidad (único sitio con nombres, colores, logos, redes)
pipeline/         scripts Python (numerados en el orden del flujo) + comun.py
remotion/         composiciones: Intro|Disclaimer|Catalogo (Parte), Grueso/GruesoGraficos,
                  Short, Linkedin, Lamina, Miniatura · escenas en src/escenas · scripts/qa.mjs
videos/           publicDir de Remotion: SOLO lo que Remotion lee (cortes, JSON, broll, música, logos)
  _shared/        music/, logos/, fx/, marca/, obj/, descripciones/, plantillas/
  <id>/           plan.md, guia_grabacion.md, partes.json, <parte>.json, shorts.json, …
references/       manuales de apoyo (ver Decision Gates)
raw/<id>/         brutos copiados de Drive + intermedios pesados (fuera de git)
out/              renders, qa/, miniatura/, entregas/<id>/ (fuera de git)
```

## Flujo (resumen)

| Paso | Qué | Comando clave |
|------|-----|---------------|
| 0 | Guion y guía de grabación | plantillas en `videos/_shared/plantillas/` |
| 1 | Traer brutos y revisar (ffprobe, espejo, rotación) | `python pipeline/brutos.py traer <id> …` |
| 2 | Partes de cámara: sync por voz → transcribir → tomas → cortar silencios | `05_sync → 02 → 03 → 01` (+ `alinear.py` solo) |
| 3 | Concepto y escenas ancladas a la voz | `lectura.py → historial.py → <parte>.json → qa.mjs` |
| 4 | Bloque de pantalla (grueso): prep, pixelado, zooms, render híbrido | `04_grueso → 06_zooms → render_grueso` |
| 5 | B-roll, miniatura (Higgsfield + Remotion) y renders por parte | `out/lote_<id>.cmd` |
| 6 | Montaje final a −14 LUFS con capítulos y descripción | `07_final.py` |
| 7 | Vertical + LinkedIn (Shorts/Reels/TikTok/4:5, carrusel PDF) | `09_shorts.py → 10_linkedin.py` |
| 8 | Cerrar: registrar historial e informe | `historial.py registrar <id>` → `LEEME_<id>.md` |

Variantes: anuncio corto (`promo01`, vertical denso con `cola`) y anuncio animado con
locución (`anuncio01`, sin cámara, paneles 3D). Detalle comando a comando: `references/flujo.md`.

## Decision Gates

| Necesitas… | Lee |
|------------|-----|
| Ejecutar el flujo completo o un paso concreto con sus comandos | `references/flujo.md` |
| Elegir escenas del catálogo (`type`), capas, fondos, estilo y SFX | `references/escenas.md` |
| Diseñar: apertura/gancho, CTA, cierre, fichas de cristal, iconos 3D, fondos animados | `references/diseno.md` |
| Miniatura, Higgsfield, voz ElevenLabs, logos, modelos locales, entorno | `references/herramientas.md` |
| Depurar un fallo conocido o evitar un pozo ya pisado | `references/reglas-tecnicas.md` |

## Output Contract

Cada proyecto termina con:

1. **YouTube horizontal** (1920×1080, −14 LUFS): partes montadas, capítulos válidos,
   descripción con fuentes y miniatura.
2. **Vertical** → 3 versiones con textos distintos: YouTube Shorts (con música),
   Instagram Reels y TikTok (sin música; el usuario la pone al publicar).
3. **LinkedIn**: vídeo 4:5, carrusel PDF, imagen suelta y texto del post.
4. Todo en `<DRIVE>/final/<id>/` (`shorts/`, `linkedin/`) e informe `out/entregas/<id>/LEEME_<id>.md`:
   qué hay, qué revisar, créditos gastados y textos listos para pegar. **La publicación es manual.**
