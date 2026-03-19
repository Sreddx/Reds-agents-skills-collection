---
name: trading-agent-evaluator
description: Evalúa y compara resultados de agentes de trading (métricas, riesgo, consistencia) y genera reportes claros.
version: 0.1.0
author: rojas
tags: [trading, evaluation, metrics, risk, reporting]
---

# Trading Agent Evaluator

## Propósito
Tomar reportes de agentes (backtest/paper/live-sim) y producir:
- Validación de consistencia
- Comparación entre agentes
- Recomendaciones para mejorar (parámetros, riesgo, instrumentación)

## Límites (No-hará)
- No acepta métricas sin periodo/fees.
- No concluye superioridad con muestras pequeñas (lo dice explícitamente).

## Entradas / Salidas
- Entradas: reportes estructurados (ver formato estándar), baseline, constraints.
- Salidas: resumen + comparación + alertas de riesgo + próximos experimentos.

## Workflow / Flujo
1) Normalizar periodos/mercados y asegurar comparabilidad.
2) Revisar PnL vs drawdown y distribución de retornos.
3) Penalizar sobreajuste (demasiados params, resultados frágiles).
4) Proponer experimentos A/B (cambios mínimos) y métricas objetivo.

## Criterios de calidad
- Reporte entendible para no expertos
- Señala incertidumbre
- Prioriza riesgo y supervivencia

## Ejemplos
- Comparar 3 bots: momentum vs mean reversion vs grid; recomendar cuál iterar según DD.
