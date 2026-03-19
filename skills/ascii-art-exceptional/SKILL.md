---
name: ascii-art-exceptional
description: Genera arte ASCII excepcional desde una imagen o una descripción, con control de estilo y legibilidad.
version: 0.1.0
author: rojas
tags: [ascii, art, image, typography, rendering]
---

# ASCII Art Excepcional

## Propósito
Convertir **una imagen** (preferido) o **una descripción** en arte ASCII de alta calidad, legible y con estética cuidada.

## Límites (No-hará)
- No afirmar fidelidad perfecta: el ASCII es una aproximación.
- No inventar detalles cuando el input es ambiguo (pedir aclaración).

## Entradas / Salidas
- Entradas:
  - Imagen (png/jpg/webp) o
  - Descripción + estilo deseado
- Salidas:
  - Bloque de texto ASCII (monoespaciado)
  - Variantes (tamaño/contraste) si se solicita

## Workflow / Flujo
1) Aclarar constraints: ancho objetivo (p.ej. 80/120/200), fondo (claro/oscuro), densidad, charset.
2) Si hay imagen: decidir preproceso conceptual (recorte, escala, contraste) y elegir charset (p.ej. ` .:-=+*#%@`).
3) Generar 2–3 versiones: (a) "fiel", (b) "alto contraste", (c) "estilizada"; luego seleccionar la mejor según criterio del usuario.

## Criterios de calidad
- Legible a distancia (macro-forma clara)
- Buen manejo de contraste (no “todo @@@@”)
- Respeta el ancho solicitado
- Sin líneas rotas raras; padding consistente

## Ejemplos
### Desde descripción
Input: “Un gato sentado mirando a la luna, estilo minimalista, 80 columnas, fondo oscuro.”
Output: (borrador)
```
      /\_/\
     ( o.o )     _
      > ^ <   .-(_)-.
               \   /
                `~`
```

### Desde imagen
- Pide: ancho (80/120/200) y si quieres modo "fiel" o "posterizado".
