# Guía de diseño e inspiración

> Consultar ANTES de componer escenas, fondos, fichas, miniaturas u outros (paso 3 del flujo: concepto).
> Cada fuente está verificada y mapeada a una decisión concreta del pipeline.
> Regla general: la inspiración se ADAPTA a la paleta y tipografías de `marca/` y al estilo de `escenas.md`
> (3D minimalista); nunca se copia un estilo ajeno tal cual.

## Cómo usarla (paso 3, concepto)

1. `lectura.py` → bloques y frases clave del guion.
2. Clasifica la intención de cada bloque (apertura/gancho, problema, cifra, proceso, cierre/CTA) y busca la
   sección de esta guía que cubre esa intención.
3. Abre el sitio, filtra por la categoría indicada, elige UNA referencia por bloque (máx. 3 referencias nuevas
   por vídeo) y anótala en `concepto.md` con su URL.
4. Adapta a `marca.json` (color, tipografías) y al catálogo de escenas (`escenas.md`); no imitar píxel a píxel.
5. QA con `qa.mjs` en 16:9 Y 9:16 y mirar los fotogramas antes de renderizar.

---

## 1. scrolltide.co — movimiento y fondos animados

**Qué es**: librería curada de plantillas web «scroll-driven» con prompt + código fuente completo. Al escribir
esto: 121 plantillas, 66 componentes, 55 shaders, 19 secciones y 14 pantallas UI. Stack: React, Three.js, WebGL,
GSAP, Framer Motion, Lenis. Suscripción de pago para descargar; navegable gratis como referencia.

**Úsalo para**:
- **Fondos animados** de escenas a pantalla completa (`"fondo"` del guion): sus categorías de shader — mesh
  gradients, aurora, halftone, swirl, flame — son el vocabulario visual para `aurora` y para variar el fondo
  entre vídeos seguidos (lo que pide `historial.py`). Muchos se ofrecen como MP4 o still: candidatos directos a
  b-roll o fondo de anuncio (descargar, no recrear).
- **Lenguaje de movimiento** para Remotion: parallax por capas, tilt 3D, reveal de tarjetas, cuándo acelerar y
  decelerar (easing orgánico, nunca lineal — misma regla que los fondos con velocidad variable).
- La escena `apertura` de anuncio (mancha orgánica + titular cinético) bebe de este lenguaje de transiciones
  orgánicas.

**Regla**: un fondo nuevo se prueba primero con `qa.mjs` en 16:9 y 9:16; si roba atención del titular, se baja
intensidad o se cambia. El fondo sirve a la voz, nunca compite con ella.

## 2. styles.refero.design — sistemas de diseño reales, legibles por IA

**Qué es**: más de 2.000 design systems de productos reales destilados en formato `DESIGN.md` (colores,
tipografía, spacing, componentes), pensados para pegarlos en agentes (Cursor, Claude Code, Codex, v0, Lovable).

**Úsalo para**:
- **Definir o refrescar `marca/marca.json`**: elegir paleta y pareja tipográfica con criterio — busca un producto
  del mismo sector que el cliente y mira sus tokens reales en vez de inventar.
- **Coherencia entre escenas**: mismas escalas de tamaño entre `callout` / `lineup` / `revealStat`; un solo color
  de acento en todo el vídeo.
- **Recrear la app del cliente en paneles 3D** (anuncios tipo `anuncio01`): abre el DESIGN.md del producto real
  como referencia de su UI ( radios, jerarquía, densidad ) y transládalo a `Panel3D`.

**Regla**: los valores tomados de un DESIGN.md se normalizan a la marca (acento de `marca.json`, tipografías
propias) antes de entrar en escenas. Es referencia de criterio, no copia de tokens.

## 3. supahero.io — aperturas y ganchos (hero sections)

**Qué es**: colección curada de hero sections de sitios reales (Stripe, Linear, Figma, Tesla, Raycast, Notion…).
Cada hero = kicker + titular + subtítulo + CTA compuestos en un solo primer impacto.

**Úsalo para**:
- **`orbOpen` y `apertura`**: la jerarquía kicker → titular → apoyo de un hero es exactamente la de una apertura
  de capítulo o anuncio.
- **Los primeros 2,8 s de un short**: el gancho ES un hero vertical — un titular, un solo foco visual, cero ruido.
- **Miniatura**: composición de primer impacto (titular grande + un punto de atención; la zona izquierda oscura
  ya la pide el flujo de miniatura en `herramientas.md`).

**Regla**: un titular de hero no supera ~7 palabras; si no cabe, se parte en dos líneas con el acento en la
palabra clave (en vertical, líneas ≤ 17 letras).

## 4. cta.gallery — llamadas a la acción

**Qué es**: galería curada de CTAs clasificados por tipo: botón, call-to-buy, descarga, formulario, modal,
newsletter, pricing/suscripción; filtrable por industria.

**Úsalo para**:
- **`outro` y cierre de shorts**: «suscríbete» + `tumarca.com` con la jerarquía de un buen CTA — una acción
  principal, texto breve, botón con peso visual.
- **Escenas de anuncio con botón** (`pago`, `final`): proporciones y estados de botón que sí parecen pulsables
  (relleno sólido o acento, esquinas coherentes con el DESIGN.md del producto).
- **Texto de miniatura**: piensa en él como un CTA de 3–5 palabras, no como un título de sección.

**Regla**: un CTA por entregable. Dos acciones simultáneas = ninguna.

## 5. footer.design — cierres y end screens

**Qué es**: galería curada de footers reales clasificados por estilo (typographic, small type, illustrative,
grid, flat, animated, cards, bright, dark, large type, bold), con selección Editor's Choice.

**Úsalo para**:
- **End screen / cierre del vídeo**: apilado de redes (las de `canal.json`), enlaces y wordmark con la estructura
  de un buen footer — el cierre de un vídeo es su footer.
- **Última lámina del carrusel LinkedIn**: es el «footer» del documento; misma jerarquía.
- **`lowerThird` y rótulos de crédito**: jerarquía tipográfica pequeña y limpia de los estilos small type/flat.

**Regla**: en cierres y créditos, la tipografía de etiquetas (mono) para datos y la de titulares para el nombre;
nunca al revés.

## 6. navbar.gallery — barras y rótulos horizontales

**Qué es**: ~550 barras de navegación de sitios reales clasificadas en 9 tipos: static/sticky, dropdown/flyout,
mega menu, sidebar, search bar, announcement bar, fullscreen, breadcrumbs y progress bar.

**Úsalo para**:
- **Rótulo de capítulo/parte y `lowerThird`**: la composición de una navbar estática — peso a la izquierda,
  texto corto, mucha respiración.
- **Barra de anuncio** (announcement bar) → banner de gancho en vertical: una línea, alto contenido, contraste.
- **Escena `buscador`** (barra que se escribe sola): mira sus search bars reales para proporciones y detalle
  (lupa, placeholder, botón).
- **Indicadores de progreso** (breadcrumbs/progress) en shorts cuando el guion tiene pasos (`pasos`).

**Regla**: una barra horizontal en vídeo no convive con el subtítulo a la misma altura; si aparecen juntas, la
barra sube y el subtítulo se queda en y≈1420.

## 7. 3dicons.co — objetos 3D gratuitos para escenas

**Qué es**: más de 1.500 renders 3D hechos a mano (≈200 iconos, 10M+ descargas), open source con colecciones
premium; color y ángulo editables online y archivo Figma disponible. By @realvjy.

**Úsalo para**:
- **Objetos 3D de escenas** (`videos/_shared/obj/`): alternativa LOCAL y GRATUITA a los objetos de Higgsfield
  (`gpt_image_2_5`, ≈ 6,5 créditos): descarga el PNG y ya viene con alfa limpia — 0 créditos, sin espera, sin
  recorte con scipy. Úsalo por defecto cuando el objeto sea genérico (calendario, gráfico, cohete, diana, buzones,
  trofeos…); reserva Higgsfield para objetos específicos del producto.
- **Icono protagonista** de una escena `pasos` o ficha de `lineup` cuando la marca no tiene logo.

**Regla**: un solo estilo de render por vídeo (una colección); no mezclar familias visuales. Verificar la
licencia en la ficha de cada colección (V1 open source; premium requiere compra).

## 8. glass.samasante.com — cristal líquido (liquid glass)

**Qué es**: «Liquid Glass» estilo Apple para la web: lente React que refracta el DOM en vivo, real en Safari,
Firefox y Chrome (MIT, sin dependencias). Referencia viva y ejecutable de cómo se ve el cristal bien hecho.

**Úsalo para**:
- **Fichas de `lineup` y callouts**: el «bloque de cristal» del estilo propio — blur + borde fino + brillo
  superior; el sitio sirve para calibrar el ojo sobre proporciones de blur/borde/highlight.
- **`lowerThird`**: recuerda la regla técnica — fondo oscuro de marca detrás; el cristal puro no se lee sobre
  pantallas claras.
- **`Panel3D` de anuncios**: vidrio oscuro con reflejo, no negro plano; es lo que hace que un panel «flote».

**Regla**: cristal = blur 8–16 px + borde 1 px blanco al 15–25 % + highlight superior. Si el texto deja de
leerse, se oscurece el relleno del panel; nunca se agranda la tipografía para compensar.

---

## Mapa rápido intención → fuente

| Intención del bloque | Fuente | Escena/ pieza afectada |
|----------------------|--------|------------------------|
| Abrir / gancho | supahero.io | `orbOpen`, `apertura`, gancho de short, miniatura |
| Cifra que impacta | scrolltide.co (impacto visual) | `revealStat`, `bars`, `rise` |
| Proceso / pasos | navbar.gallery (breadcrumbs), 3dicons.co | `pasos`, `buscador` |
| Problema / molestia | navbar.gallery (announcement) | `notificacion`, banner de gancho |
| Fichas / comparación | glass.samasante.com | `lineup`, fichas de concepto |
| Cerrar / CTA | cta.gallery + footer.design | `outro`, end screen, cierre de short, última lámina LinkedIn |
| Definir marca / coherencia | styles.refero.design | `marca.json`, escalas tipográficas |
| Fondos y movimiento | scrolltide.co (shaders) | `"fondo": "aurora"…`, b-roll, transiciones orgánicas |
