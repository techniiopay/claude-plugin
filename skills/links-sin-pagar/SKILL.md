---
name: links-sin-pagar
description: "Lista los links de cobro de iion pay que todavía no se han pagado, con su monto, su vencimiento y su enlace. Requiere el permiso de ventas. Usa esta skill cuando el dueño pregunte qué links no han pagado, qué cobros siguen pendientes o qué links vencen pronto."
---

# Links sin pagar

Esta skill lista cobros pendientes. Usa las herramientas de ventas, por eso requiere el permiso de ventas.

## Requisito

Si `list_charges` no existe o responde que el token no tiene permiso de ventas, detente. Di al dueño que debe autorizar el permiso de ventas en el panel de iion pay. No pidas ni pegues ningún token.

## Herramientas

1. `list_charges` para listar los cobros del más reciente al más antiguo. Usa `limit` de 20 o menos y pide la página siguiente con `offset` solo si el dueño lo pide.
2. `get_charge` con el `id` de un cobro, solo si el dueño pide el detalle de uno.

Un cobro sin fecha de pago y sin cancelar está pendiente. Confirma el estado con `get_charge` antes de decir que un cobro está pendiente.

## Cómo responder

1. Di cuántos links siguen sin pagar y su monto total por moneda.
2. Muestra cada link en una lista corta: monto, moneda, fecha de vencimiento y enlace de pago (`collectUrl`).
3. Ordena los links por vencimiento, del más cercano al más lejano.

## Reglas

- Esta skill no cancela cobros. Si el dueño pide cancelar uno, usa `cancel_charge` solo con su confirmación explícita para ese cobro.
- No crees cobros nuevos desde esta skill.
- No cambies montos ni monedas. Muestra los datos tal como llegan.
