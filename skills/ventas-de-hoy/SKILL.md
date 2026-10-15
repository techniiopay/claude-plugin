---
name: ventas-de-hoy
description: "Muestra cuánto vendió el comercio hoy en iion pay: ingresos, gastos, neto y cobros por estado. Solo consulta. Usa esta skill cuando el dueño pregunte cuánto vendió hoy, cómo va el día o cuántos cobros tiene."
---

# Ventas de hoy

Esta skill solo consulta datos. No crea ni cambia cobros.

## Herramientas

1. `business_summary` con el período de un día (hoy): ingresos, gastos, neto e IVA anotado.
2. `business_payin` con `from` y `to` iguales a hoy: cobros del día, por estado y con un total por moneda.
3. `business_balance` solo si el dueño pregunta por su saldo.

Ejecuta las herramientas 1 y 2 en el mismo turno. No esperes una respuesta para lanzar la otra.

## Cómo responder

1. Di primero el total vendido hoy, con su moneda.
2. Después di cuántos cobros se completaron y cuántos quedan pendientes, según el estado que devuelve `business_payin`.
3. Si hay gastos registrados, muéstralos en una línea aparte.
4. Si una herramienta falla, dilo. No estimes el dato que falta.

## Reglas

- Usa solo los datos que devuelven las herramientas. No inventes cifras.
- Los importes de `business_payin` vienen separados por moneda. No los sumes ni los conviertes.
- Si el dueño pide otro día, repite las herramientas con ese período.
