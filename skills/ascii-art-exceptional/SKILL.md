---
name: ascii-art-exceptional
description: Genera arte ASCII de alta fidelidad desde una imagen (preferido) o una descripción, con control de estilo y legibilidad.
version: 0.2.0
author: rojas
tags: [ascii, art, image, typography, dithering, cli]
---

# ASCII Art Excepcional

## Propósito
Convertir **una imagen real** (preferido) o **una descripción** en arte ASCII **que sí se parezca** al input: buena silueta, contraste, proporción y detalle.

## Límites (No-hará)
- No prometer “idéntico”: el ASCII es compresión.
- Si no hay imagen (solo descripción), el resultado es interpretativo.
- Si el sujeto es muy complejo o la resolución es baja, se pedirá:
  - recorte (ROI), o
  - mayor ancho (120/160/200), o
  - versión por zonas (cara / fondo).

## Entradas / Salidas

### Entradas
- Imagen (png/jpg/webp) **idealmente** con:
  - sujeto claro, buen contraste
  - sin motion blur
- Parámetros:
  - ancho objetivo: 80 / 120 / 160 / 200
  - fondo: claro u oscuro
  - estilo: fiel | alto-contraste | posterizado | edge/contornos

### Salidas
- ASCII monoespaciado (texto)
- 2–3 variantes con diferente preproceso
- (Opcional) versión “ANSI color” si el usuario lo quiere

## Workflow / Flujo (imagen)

### Paso 0 — Asegurar contexto
Preguntar SIEMPRE:
- ancho objetivo
- fondo (claro/oscuro)
- prioridad: parecido (fiel) vs estilo (artístico)

### Paso 0.5 — Describir y validar (obligatorio)
Antes de convertir a ASCII, **describe la imagen con tus palabras** (sujeto, escena, elementos clave, iluminación, fondo) y pide al usuario que confirme/corrija.
- Si el usuario dice que no se parece: pide recorte/zoom o una foto con más resolución/contraste.
- No sigas al render ASCII hasta que el usuario diga “sí, esa es la imagen / correcto”.

### Paso 1 — Preproceso (clave para que se parezca)
Aplicar mentalmente/operativamente estas transformaciones (en este orden):
1) **Recorte** al sujeto (evita que el fondo destruya el rango dinámico)
2) **Escala** manteniendo aspecto
3) **Corrección de aspecto ASCII** (caracteres no son cuadrados)
4) **Contraste/Gamma** (subir micro-contraste)
5) **Dithering** (Floyd–Steinberg u ordenado) para preservar detalle

### Paso 2 — Render ASCII (recomendado vía CLI)
Preferir herramientas probadas cuando esté disponible el entorno:
- `chafa` (recomendado: controles finos de tamaño, **font-ratio**, y dither)
- `jp2a` (clásico, soporta `--background=dark|light`, `--chars`, `--width/--size`)

Notas verificadas en documentación:
- `chafa` soporta `--font-ratio` (por defecto 1/2 en modo símbolos) y `--dither` con sinónimos: `fs` ≈ Floyd–Steinberg (diffusion). Fuente: man page de chafa.
- `jp2a` soporta `--background=dark|light` y `--chars=...` y `--width/--size`. Fuente: man page de jp2a.

Comandos de referencia (ajustar al caso):

**chafa (fidelidad, grises, dither FS, ratio correcto)**
```bash
chafa -c none --symbols=ascii --font-ratio 1/2 --dither fs -s {{WIDTH}}x {{IMAGE}}
```

**chafa (más detalle: subir work + dither grain)**
```bash
chafa -c none --symbols=ascii --font-ratio 1/2 --work 7 --dither diffusion --dither-grain 4x4 -s {{WIDTH}}x {{IMAGE}}
```

**jp2a (fidelidad simple + background correcto)**
```bash
jp2a --background=dark --width={{WIDTH}} {{IMAGE}}
```

**jp2a (charset custom, más rango tonal)**
```bash
jp2a --background=dark --width={{WIDTH}} --chars=" .'\"`^,:;Il!i~+_-?][}{1)(|\\/tfjrxnuvczXYUJCLQ0OZmwqpdbkhao*#MW&8%B@$" {{IMAGE}}
```

### Paso 3 — Selección de charset
- Para fidelidad: ` .:-=+*#%@`
- Para estilo “bold”: ` .'`^",:;Il!i~+_-?][}{1)(|\/tfjrxnuvczXYUJCLQ0OZmwqpdbkhao*#MW&8%B@$`

### Paso 4 — Post-proceso
- Recortar espacios laterales
- Asegurar que el bloque no exceda el ancho
- Entregar 2–3 variantes nombradas: FIEL / CONTRASTE / ESTILIZADA

## Workflow / Flujo (solo descripción)
1) Pedir: sujeto, composición, mood, y “nivel de detalle” (minimalista vs denso)
2) Hacer 2 bocetos (silueta) y 1 versión detallada
3) Iterar con el usuario (esto NO puede “parecerse a una foto” literal)

## Criterios de calidad (smoke check humano)
- Silueta reconocible en 2–3 segundos
- Contraste equilibrado (sin empastar)
- Proporción correcta (no “aplastado”)
- No excede ancho solicitado

## Ejemplos

### Desde imagen (checklist)
- ¿Quieres 80/120/160/200 columnas?
- ¿Fondo oscuro o claro?
- ¿Prefieres fidelidad o estilo?

### Desde descripción (minimalista)
Input: “Un gato sentado mirando a la luna, minimalista, 80 columnas, fondo oscuro.”
Output:
```
      /\_/\
     ( o.o )
      > ^ <        _
                .-(_)-.
                 \   /
                  `~`
```
