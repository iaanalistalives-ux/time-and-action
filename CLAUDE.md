# CLAUDE.md — Time & Action (time-and-action)

Las reglas generales de trabajo están en el repo privado `lives-instrucciones` (CLAUDE.md). Este archivo solo describe esta herramienta.

## Qué hace
Time & Action (T&A) de cada pedido por cliente: hitos de pre-producción y etapas de producción, plan contra real, y el documento que se envía al cliente.
- `carga.html` — **llenado** (Comercial):
  - Inicio con tarjetas de clientes; lista de T&A del cliente con estado, plan original (congelado o no) y avance; regla 500.
  - Formulario: datos del pedido (código, temporada, estilo, cantidad, colores, tela, confirmación, ex-factory, estado, ex-factory real), hitos en dos secciones (pre-producción y proceso), cada uno de una fecha o de inicio y fin, con real, "solo interno" y nota; nota interna y nota al cliente.
  - Plantillas: calcula las fechas desde el ex-factory con los días de la plantilla (salen de T&A anteriores, no son leadtimes oficiales de PCP); "Guardar como plantilla del cliente".
  - **Congelar plan original**: las fechas de ese momento quedan como plan original; después cada cambio sube la revisión.
  - Borrador local cada 20 s (7 días).
- `index.html` — **consulta y documento**: tarjetas con franja por etapa, filtros de cliente y estado (Anulado nunca se muestra), regla 500. El T&A abierto se muestra como 3 hojas A4: resumen y recorrido, línea de tiempo (Gantt) y programa de hitos con firmas. Inglés o español; versión "Para el cliente" (oculta lo interno) o "Interna" (plan original, atraso, holgura, notas). "Descargar PDF / Imprimir" y Excel LIVES. `#ta=<id>` abre un T&A; `#cliente=<SIGLA>` filtra.

## Base de datos (Supabase lives-datos)
- Una sola puerta: `ta_api(p_accion, p)`.
  - Leer: `permiso`, `clientes`, `plantillas`, `pedidos`, `pedido`.
  - Registrar: `guardar`, `congelar`, `guardar_plantilla`.
- Tablas: `ta_pedidos`, `ta_pedido_hitos`, `ta_plantillas`, `ta_plantilla_hitos`; clientes de `pc_clientes` (los mismos de Perfil de Clientes).
- No usa Storage.

## Quién la usa
Según las entradas por el portal de los últimos 30 días al 21/10/2026: 2 personas (Comercial y Datos e IA). Al 21/10/2026 todavía no hay T&A guardados.

## Quién puede registrar
- Lo decide la base: `ta_puede_ver` y `ta_puede_editar` (dentro de `ta_api`). Sin permiso de editar, la pantalla de llenado queda en solo consulta ("Solo para quien registra").
- A Sistemas: `ta_puede_editar` es la pieza que reemplazan con su seguridad.

## Pendientes vistos al escribir esto (21/10/2026)
- En `index.html` el % de avance y los vencidos cuentan también los hitos "solo interno" (filtro sin efecto en `resumen()`), aunque la versión cliente no los muestre.
