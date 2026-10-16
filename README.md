# Videoskill

> Tú grabas. El agente edita: corta silencios y tomas falsas, sincroniza cámaras por la voz,
> anima escenas 3D con Remotion y entrega YouTube, Shorts, Reels, TikTok y LinkedIn a −14 LUFS.
> 100 % marca blanca: tu logo, tus colores. Incluye guía de inspiración de diseño con 8 fuentes verificadas.

Skill en formato estándar (`SKILL.md` + `references/`), basado en
[Skill-Editor-IA](https://github.com/brianacevedo2001-lab/Skill-Editor-IA) de
[brianacevedo2001-lab](https://github.com/brianacevedo2001-lab) (MIT).

## Qué cambia respecto al original

- **`CLAUDE.md` → `SKILL.md`**: manual del agente reestructurado en formato skill con frontmatter
  (`name`, `description`, `license`, `metadata`) y **carga progresiva**: el orquestador es corto y el
  detalle vive en `references/` — el agente lee solo lo que necesita para el paso actual.
- **Nueva guía de diseño** (`references/diseno.md`): 8 fuentes de inspiración verificadas y mapeadas a
  decisiones concretas del pipeline:
  | Fuente | Para qué se usa aquí |
  |--------|----------------------|
  | [scrolltide.co](https://scrolltide.co) | Fondos animados y vocabulario de movimiento (shaders, aurora, mesh) |
  | [styles.refero.design](https://styles.refero.design) | Design systems reales legibles por IA → `marca.json` y coherencia |
  | [supahero.io](https://supahero.io) | Aperturas, ganchos de short y composición de miniatura |
  | [cta.gallery](https://cta.gallery) | Outros, botones de anuncio y texto de miniatura como CTA |
  | [footer.design](https://footer.design) | End screens, cierre del carrusel LinkedIn, créditos |
  | [navbar.gallery](https://navbar.gallery) | Lower thirds, rótulos de capítulo, banner de gancho |
  | [3dicons.co](https://3dicons.co) | Objetos 3D gratuitos (alternativa local a Higgsfield, 0 créditos) |
  | [glass.samasante.com](https://glass.samasante.com) | Bloques de cristal y paneles 3D (liquid glass) |
- **Objetos 3D sin créditos**: flujo preferente con 3dicons.co antes de gastar en Higgsfield.
- `.gitignore` ampliado (`.env` protegido).

Todo el pipeline, las 90+ reglas técnicas y el catálogo de escenas del original se conservan
íntegros en `references/`.

## Estructura

```
SKILL.md                  manual del agente (orquestador): reglas duras, flujo resumido, decision gates
references/
  flujo.md                flujo completo paso a paso con comandos (proyecto01, promo01, anuncio01)
  escenas.md              catálogo de escenas (type), capas, fondos y estilo
  diseno.md               guía de inspiración de diseño (8 fuentes verificadas)
  herramientas.md         miniatura, Higgsfield, voz, logos, modelos locales, entorno
  reglas-tecnicas.md      registro de fallos de producción y sus soluciones (leer SIEMPRE)
marca/                    identidad: marca.json, canal.json, proyecto.json + logos
pipeline/                 26 scripts Python numerados en el orden del flujo
remotion/                 composiciones, escenas, fondos 3D, scripts de QA y logos
videos/                   publicDir de Remotion: guiones JSON, plantillas, música, fx
```

## Puesta en marcha

> Probado en Windows 11 + GPU NVIDIA (RTX 4060, CUDA 12).

```bash
# 1. Clonar
git clone https://github.com/toxykdude/videoskill.git
cd videoskill

# 2. Python
python -m venv .venv
.venv\Scripts\pip install -r requirements.txt
.venv\Scripts\pip install torch torchaudio torchvision --index-url https://download.pytorch.org/whl/cu128

# 3. ffmpeg y Remotion
winget install Gyan.FFmpeg
cd remotion && npm install && cd ..

# 4. Tu identidad: rellena marca/*.json y pon marca/isotipo.png + marca/logotipo.png
#    (cópialos también a videos/_shared/marca/)

# 5. Librerías de efectos y logos
.venv\Scripts\python pipeline/sfx_generar.py
node remotion/scripts/logos.mjs
```

Y luego, con tu agente en la carpeta:

```
> Tengo los brutos del episodio 1 en Drive. Edítalo y sácame los shorts.
```

## Licencias

Este proyecto: MIT (ver `LICENSE`; deriva de Skill-Editor-IA, también MIT).
Terceros: [Remotion](https://www.remotion.dev/license) · Robust Video Matting (GPL-3.0, vía torch.hub) ·
MMS_FA de torchaudio · Simple Icons / Lobe Icons / SVGL para logos · las 8 fuentes de diseño citadas en
`references/diseno.md` pertenecen a sus autores y se usan como referencia/inspiración.
