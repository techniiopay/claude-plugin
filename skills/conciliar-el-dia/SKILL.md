---
name: conciliar-el-dia
description: "Concilia las ventas de un día del comercio en iion pay: lo vendido, lo liquidado, la comisión, los reversos y la diferencia entre ambos. Solo consulta. Usa esta skill cuando el dueño pregunte si le cuadra el día, cuánto le llegó de un día o por qué no coincide un monto."
---

# Conciliar el día

Esta skill compara lo vendido con lo liquidado. Solo consulta datos.

## Herramientas

1. `business_reconciliation` con el día pedido. Sin fechas, usa el mes en curso; para un día, pasa `from` y `to` iguales.
2. `business_settlements` con el mismo día, si el dueño pide el detalle de los abonos.
3. `business_funds_summary` solo si el dueño pregunta dónde está hoy el dinero.

## Cómo responder

1. Di lo vendido, lo liquidado y la diferencia, por moneda.
2. Explica la diferencia con los datos que devuelve la herramienta: comisión, reversos o ventas cuyo cobro no cuadra.
3. Si la diferencia es cero, dilo en una frase.
4. Si la herramienta no devuelve un dato, dilo. No lo calcules por tu cuenta.

## Reglas

- Las fechas son días de Bogotá. Usa el día que pidió el dueño, no el día UTC.
- La conciliación no suma ni convierte monedas. Muestra cada moneda por separado.
- La herramienta acepta hasta 31 días. Para un período mayor, pide al dueño que lo divida.
- Esta skill no mueve dinero ni cambia liquidaciones.
