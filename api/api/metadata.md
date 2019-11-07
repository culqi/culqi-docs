---
id: metadata
title: Metadata
---

La metadata es una manera útil de guardar información adicional de manera estructurada en un objeto. Todos los recursos de Culqi, incluyendo Tokens, Cargos, Planes, Suscripciones, Devoluciones, Tarjetas, Clientes tienen un parámetro metadata que puede ser utilizado para reflejar información clave de tu negocio que quieras relacionar con las operaciones.

Por ejemplo, en el parámetro metadata del recurso Clientes podrías guardar el nombre completo de tu cliente, su número de documento de identidad y su fecha de nacimiento. La metadata no es utilizada por Culqi para analizar, realizar o rechazar un cargo.

> El atributo metadata no puede tener más de 20 objetos clave-valor. Adicionalmente, la clave no podrá exceder de 30 caracteres y el valor de 200 caracteres.

##  Ejemplos de Metadata

Por ejemplo, en la petición para crear un cargo al API, podrías enviar a través del objeto metadata los valores definidos anteriormente:

```json
{
   "amount":200,
   "currency_code":"PEN",
   "source_id":"tkn_live_189fqQ2eZvKYlo2",
   "description":"Apple Watch Series 20",
   "installments":6,
   "capture":true,
   "metadata":{
      "order_id":"204055",
      "user_id":"4625",
      "user_details":"46734527"
   }
}
```

Aquí te presentamos algunas sugerencias para los valores de metadata: