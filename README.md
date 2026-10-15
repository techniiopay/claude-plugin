# Plugin de iion pay para Claude Code

Consulta tu negocio de iion pay desde Claude Code. Pregunta por tus ventas del día,
tus links sin pagar o la conciliación de un día, sin pegar ningún token.

El plugin conecta Claude Code con el servidor MCP de iion pay. No instala nada en tu
computador: el servidor corre en la nube.

## Qué incluye

- **Servidor MCP remoto** `https://api.prod.niiopay.com/api/v1/mcp`, con inicio de sesión OAuth.
- **3 skills** que guían las consultas:

| Skill | Para qué sirve | Permiso |
|-------|----------------|---------|
| `ventas-de-hoy` | Cuánto vendiste hoy, cobros por estado y gastos | Solo consulta |
| `conciliar-el-dia` | Lo vendido frente a lo liquidado, con comisiones y reversos | Solo consulta |
| `links-sin-pagar` | Links de cobro que todavía no se han pagado | Ventas |

## Instalación

Necesitas Claude Code. Abre una terminal y ejecuta:

```bash
claude plugin marketplace add techniiopay/claude-plugin
claude plugin install iion-pay@iion-pay
```

Luego abre Claude Code, escribe `/mcp` y elige `iion-pay`. Se abre el navegador para
iniciar sesión con tu cuenta de iion pay. Autoriza el acceso y vuelve a Claude Code.

## Permisos

Al autorizar, eliges qué puede hacer Claude con tu negocio:

- **Solo consulta (por defecto).** Claude lee tus ventas, cobros, saldo y conciliación.
  No cambia nada.
- **Ventas (opcional).** Además, Claude puede listar, consultar y cancelar cobros.
  Actívalo solo si lo necesitas. La skill `links-sin-pagar` requiere este permiso.

Puedes cambiar los permisos desde el panel de iion pay.

## Soporte

- Sitio: [iionpay.com](https://iionpay.com)

## Licencia

MIT. Ver [LICENSE](LICENSE).
