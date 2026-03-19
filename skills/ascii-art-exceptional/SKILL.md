---
name: ascii-art-exceptional
description: Genera arte ASCII de alta fidelidad desde una imagen real, especialmente retratos, con variantes optimizadas para WhatsApp y validación iterativa de parecido.
version: 0.3.0
author: rojas
tags: [ascii, art, image, typography, dithering, cli, whatsapp, portrait]
---

# ASCII Art Excepcional

## Propósito
Convertir **una imagen real** en arte ASCII que **sí conserve identidad visual**: silueta, sombras, proporción facial, ojos, nariz, boca, barba/cabello y separación sujeto/fondo. Debe priorizar **parecido** sobre estilización, especialmente en **selfies/retratos** y salidas para **WhatsApp**.

## Límites (No-hará)
- No prometer “idéntico”: ASCII siempre comprime.
- No fingir que una salida pobre es “fiel”. Si no se parece, hay que **iterar**.
- No mezclar demasiados objetivos a la vez. Elegir primero: **parecido**, **legibilidad móvil**, o **estilo**.
- Si no hay acceso a la imagen real (solo preview, screenshot o descripción), reconocer que la fidelidad estará limitada.

## Prioridades de calidad
Orden obligatorio:
1. **Reconocibilidad del sujeto**
2. **Proporción correcta del rostro/cuerpo**
3. **Separación sujeto/fondo**
4. **Legibilidad en el medio destino** (WhatsApp, terminal, README, etc.)
5. **Estilo**

## Entradas / Salidas

### Entradas
- Imagen (png/jpg/webp) idealmente original
- Parámetros mínimos:
  - ancho objetivo: 80 / 96 / 120 / 160 / 200
  - fondo: claro u oscuro
  - destino: WhatsApp | terminal | markdown | ANSI color
  - prioridad: parecido | compacto | estilizado
  - sujeto: rostro | busto | escena completa

### Salidas
- ASCII monoespaciado
- **2–4 variantes** nombradas
- Recomendación explícita de cuál usar
- Si el output no es suficientemente fiel: siguiente iteración propuesta

## Flujo obligatorio

### Paso 0 — Definir contexto de salida
Preguntar o inferir:
- destino (si es WhatsApp, preferir ancho 80–100 o bloques compactos)
- fondo claro/oscuro
- foco: selfie/cara/escena completa
- prioridad: **parecido** o **comodidad para reenviar**

### Paso 0.5 — Descripción y validación (obligatorio)
Antes de renderizar, describir la imagen con palabras:
- sujeto
- pose/ángulo
- iluminación
- fondo
- rasgos dominantes (barba, lentes, gorra, perfil, sonrisa, etc.)

Si el usuario dice “no se parece”, **no discutir**: iterar.

### Paso 1 — Decidir encuadre (crítico)
No renderizar la imagen completa por defecto. Elegir una de estas estrategias:
- **Rostro/face crop** → para parecido máximo
- **Busto** → para identidad + contexto
- **Escena completa** → solo si el fondo importa realmente

Regla: para selfies y WhatsApp, preferir **rostro** o **busto**. El fondo casi siempre destruye rango tonal útil.

### Paso 2 — Pipeline de preproceso
Aplicar este pipeline conceptual u operativo:
1. **Crop ROI** del sujeto
2. **Desaturar / grayscale**
3. **Auto-contrast** moderado
4. **Ajuste de gamma** para preservar medios tonos
5. **Sharpen/micro-contrast** suave si los rasgos se lavan
6. **Resize** manteniendo aspecto
7. **Corrección de aspecto ASCII** (font ratio)
8. **Dither** si ayuda a rescatar textura, no por costumbre

### Paso 3 — Elegir modo según objetivo

#### Modo A — Retrato fiel (recomendado para selfies)
- ancho: 96 / 120 / 160
- charset largo
- preservar medios tonos
- no exagerar contraste demasiado pronto

#### Modo B — WhatsApp compacto
- ancho: 72 / 80 / 96
- priorizar silueta + ojos + barba/boca
- recortar más fuerte
- evitar bloques demasiado anchos que rompan en móvil

#### Modo C — Bloques densos
- usar `░▒▓█` si el usuario prioriza impacto visual más que textura fina

### Paso 4 — CLI recomendada
Preferir herramientas probadas cuando existan.

## Recomendaciones verificadas
### chafa
Confirmado en docs/manpage:
- `--font-ratio` existe y en symbol mode el default es `1/2`
- `--dither fs` es alias de Floyd–Steinberg / diffusion
- `--work` aumenta precisión
- `--color-extractor median` suele dar salida más crujiente que `average`
- `--preprocess` puede mejorar contraste/colores antes de convertir

### jp2a
Usar cuando esté disponible por simplicidad y buen rango tonal en texto puro:
- `--background=dark|light`
- `--width`
- `--chars`

## Comandos de referencia

### chafa — retrato fiel, texto puro
```bash
chafa -f symbols -c none --symbols=ascii \
  --font-ratio 1/2 --color-extractor median \
  --work 7 --dither fs --dither-grain 4x4 \
  --preprocess on -s {{WIDTH}}x {{IMAGE}}
```

### chafa — retrato WhatsApp compacto
```bash
chafa -f symbols -c none --symbols=ascii \
  --font-ratio 1/2 --color-extractor median \
  --work 7 --dither ordered --dither-grain 2x4 \
  --preprocess on -s {{WIDTH}}x {{IMAGE}}
```

### jp2a — fidelidad simple
```bash
jp2a --background=dark --width={{WIDTH}} {{IMAGE}}
```

### jp2a — retrato con más rango tonal
```bash
jp2a --background=dark --width={{WIDTH}} \
  --chars=" .'\"^,:;Il!i~+_-?][}{1)(|\\/tfjrxnuvczXYUJCLQ0OZmwqpdbkhao*#MW&8%B@$" \
  {{IMAGE}}
```

## Charsets recomendados

### 1) Fiel / general
```text
 .:-=+*#%@
```

### 2) Largo / retrato realista
```text
 .'"^,:;Il!i~+_-?][}{1)(|\/tfjrxnuvczXYUJCLQ0OZmwqpdbkhao*#MW&8%B@$
```

### 3) Bloques / impacto
```text
 ░▒▓█
```

## Iteración obligatoria (clave)
Después de generar, evaluar explícitamente:
- ¿se reconoce en 2–3 segundos?
- ¿ojos/nariz/boca/barba/cabello siguen presentes?
- ¿el rostro quedó aplastado o estirado?
- ¿se perdió demasiada información en fondo o sombras?
- ¿sirve para el medio destino (WhatsApp)?

Si falla, iterar en este orden:
1. recortar más al rostro
2. subir ancho
3. cambiar charset a uno más largo
4. corregir contraste/gamma
5. cambiar dither
6. probar bloques `░▒▓█`

## Benchmarking / comparación
Si el usuario trae un ejemplo mejor de otro modelo o herramienta:
- tomarlo como **benchmark válido**
- identificar qué ganó: rango tonal, densidad, proporción, textura
- ajustar el siguiente intento para acercarse a eso
- no defender una salida mediocre

### Benchmark preferido para retratos/selfies
Si una variante previa logra mejor parecido gracias a:
- **más rango tonal**
- **más densidad de caracteres**
- **menos posterización**
- **más altura útil del rostro**

entonces úsala como referencia para iteraciones futuras.

Regla práctica:
- Para retratos, preferir salidas tipo **“primer bloque denso/tonal”** sobre versiones demasiado limpias, simétricas o iconizadas.
- Si hay duda entre una salida “bonita” y una salida “que sí se parece”, elegir la que **sí se parece**.
- Para WhatsApp, si el ancho lo permite, conservar densidad tonal antes que simplificar demasiado el charset.

## WhatsApp mode
Para WhatsApp:
- preferir ancho **72–96** si se va a pegar directo al chat
- evitar líneas excesivamente largas
- entregar una versión limpia sin explicación alrededor si el usuario quiere reenviarla
- si el retrato pierde demasiado detalle a 72–96, ofrecer versión “HD” de 120–160 y otra compacta

## Criterios de smoke check humano
Una salida pasa si:
- la silueta y el rostro son reconocibles rápido
- no parece un logo genérico ni una máscara simétrica
- mantiene volumen (frente, pómulos, barba, pelo)
- respeta el ancho pedido
- hay al menos una variante claramente usable

## Ejemplo de entrega ideal
- **FIEL** → más parecido, ancho 120
- **WHATSAPP** → más compacto, ancho 80
- **BLOQUES** → más impacto visual
- Recomendación: “Usa WHATSAPP si lo vas a pegar en chat; usa FIEL si lo vas a mandar como bloque monoespaciado grande.”
