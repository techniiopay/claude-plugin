---
name: mi-tienda
description: "Gestiona la mini tienda del comercio en iion pay: las reservas de hoy, validar y canjear un bono o una entrada, y cuántos asistentes tiene un evento. Las consultas funcionan con cualquier permiso; canjear, cancelar o anular requiere el permiso de tienda. Nunca mueve dinero. Usa esta skill cuando el dueño pregunte por sus reservas, quiera validar un código de un cliente o pregunte quién viene a un evento."
---

# Mi tienda

Esta skill consulta y opera la mini tienda del comercio. Ninguna herramienta mueve dinero:
cancelar o anular no reembolsa, y canjear no cobra.

## Requisito

- Las consultas (`store_bookings`, `store_events`, `store_vouchers`, `store_overview`) funcionan con cualquier permiso.
- Las acciones (`redemption_lookup`, `redemption_consume` y las de cancelar o anular) requieren el permiso de tienda.
- Si una acción no existe o responde que el token no tiene permiso, detente. Di al dueño que debe autorizar el permiso «Consulta y tienda» en el panel de iion pay. No pidas ni pegues ningún token.
- Si una herramienta responde que la app no está disponible o no está instalada, dilo con esas palabras y detente.

## Confirmación obligatoria

Antes de llamar una herramienta que cancela, anula o canjea, haz esto:

1. Muestra al dueño qué vas a hacer y sobre qué registro (servicio y hora, producto o entrada).
2. Pide una confirmación explícita, por ejemplo «¿Confirmas que canjeo este bono?».
3. Llama la herramienta solo si el dueño responde que sí a esa pregunta. Una respuesta ambigua no es un sí.

Estas herramientas exigen confirmación: `booking_cancel`, `membership_cancel`, `event_ticket_void`,
`event_cancel`, `voucher_void` y `redemption_consume`. Una confirmación vale para un solo registro.

## Flujo 1: reservas de hoy

1. Llama `store_bookings` con `from` y `to` iguales a hoy (YYYY-MM-DD, día de Bogotá).
2. Di cuántas reservas hay hoy (`total`).
3. Muestra cada reserva en una línea: hora de `startsAt` en hora de Bogotá, `service.name` y `status`.
4. Si `total` es mayor que las reservas recibidas, dilo y ofrece pedir más con `limit` (máximo 50).
5. Las reservas no traen el nombre ni el contacto del cliente. Si el dueño los pide, dile que los vea en el panel.

## Flujo 2: validar un bono o una entrada

1. Pide al dueño el código que muestra el cliente.
2. Llama `redemption_lookup` con ese `code`. Esta herramienta solo consulta; no canjea.
3. Muestra el resultado:
   - Bono (`kind: voucher`): `productName`, sesiones usadas de `sessionsTotal`, vencimiento (`expiresAt`) y `status`.
   - Entrada (`kind: ticket`): `eventTitle`, `ticketTypeName`, fecha (`startsAt`), `status` y si el evento está cancelado (`eventCanceled`).
4. Si el estado no permite el canje (vencido, agotado, anulado, ya usado o evento cancelado), dilo y detente.
5. Si se puede canjear, pide la confirmación obligatoria.
6. Con el sí del dueño, genera un UUID v4 nuevo y llama `redemption_consume` con el mismo `code` y ese `idempotencyKey`.
7. Si la llamada falla o no responde, repítela con el MISMO `idempotencyKey`. Nunca generes otro para el mismo canje.
8. Di el resultado. Si la respuesta trae `replay: true`, el código ya estaba canjeado con esa clave: no se canjeó dos veces.

## Flujo 3: quién viene al evento

1. `store_events` necesita el `eventId` para contar asistentes. Las herramientas no listan los eventos con su id.
2. Pide al dueño el id del evento. Está al final de la dirección de la página del evento en el panel de iion pay (Mi tienda › Eventos).
3. Llama `store_events` con ese `eventId`.
4. Di el total de entradas (`attendance.total`) y, por tipo de entrada, vendidas, usadas, anuladas y disponibles.
5. La respuesta no trae los nombres de los asistentes. Si el dueño los pide, dile que los vea en el panel.

## Reglas

- Usa solo los datos que devuelven las herramientas. No inventes cifras ni estados.
- Las fechas son días de Bogotá. Usa el día que pidió el dueño, no el día UTC.
- Los importes van en COP. No los conviertas.
- Si una acción falla, di que revise en el panel si quedó hecha antes de repetirla. La excepción es `redemption_consume`, que se repite con la misma clave.
- No canceles, anules ni canjees nada que el dueño no haya pedido.
