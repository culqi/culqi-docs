---
id: id-de-rastreo
title: ID de Rastreo
---

Cada petición al API está asociada a un identificador de rastreo. Puedes encontrar este valor en los headers de respuesta, bajo el nombre de x-culqi-tracking-id, también lo puedes encontrar en el Panel de Culqi y en el detalle de cada petición.

> Si necesitas ayuda con alguna petición en específico, brindando este ID de rastreo hará más fácil la ubicación y posterior solución del problema o incidencia.

## Ejemplo de Headers de Respuesta:

```json
{
    "date": "1476132639",
    "x-culqi-environment": "live",
    "x-culqi-tracking-id": "9283",
    "x-culqi-version": "17.01.89",
    "content-type": "application/json"
}
```

