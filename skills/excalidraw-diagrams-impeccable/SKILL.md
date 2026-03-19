---
name: excalidraw-diagrams-impeccable
description: Diseña diagramas impecables al estilo Excalidraw, fáciles de editar, y entrega siempre un PNG (con especificación editable).
version: 0.1.0
author: rojas
tags: [diagram, excalidraw, ux, architecture, flowchart]
---

# Excalidraw Diagrams Impeccable

## Propósito
Crear diagramas de calidad profesional con estética tipo Excalidraw: **claros**, **atractivos** y **editables**. Entrega final: **PNG** (siempre). Además, produce una **especificación editable** (texto/JSON/mermaid-like) para recrearlo.

## Límites (No-hará)
- No asumir el tipo de diagrama si el usuario no lo define (sugerir opciones).
- No mezclar demasiadas notaciones en un mismo diagrama.

## Entradas / Salidas
- Entradas: objetivo + audiencia + tipo de diagrama + entidades/relaciones.
- Salidas:
  - PNG (final)
  - “Blueprint” editable: lista de nodos/aristas + layout + estilos

## Workflow / Flujo
1) Identificar el tipo: flujo (flowchart), arquitectura, ERD, sequence, timeline, mapa mental, org chart, etc.
2) Definir convenciones: colores por categoría, iconografía mínima, tipografía, grid y espaciado.
3) Producir blueprint editable (nodos/aristas) y luego renderizar/exportar a PNG.

## Criterios de calidad
- Jerarquía visual clara (título, secciones, agrupaciones)
- Alineación + spacing consistente
- Etiquetas cortas y accionables
- Fácil de editar: pocos estilos, componentes reutilizables

## Ejemplos
### Solicitud
“Diagrama de arquitectura: cliente web -> API Gateway -> servicios A/B -> DB, con auth y rate-limit.”

### Blueprint (ejemplo)
- Nodes:
  - Client(Web)
  - API Gateway
  - Auth
  - Service A
  - Service B
  - DB
- Edges:
  - Client -> API Gateway
  - API Gateway -> Auth
  - API Gateway -> Service A/B
  - Service A/B -> DB

Entrega: PNG + blueprint.
