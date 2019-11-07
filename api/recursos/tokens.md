Tokenización es el proceso que utiliza Culqi para capturar de manera segura datos sensibles de tarjetas de crédito y débito directamente desde el navegador del cliente. Un token representa la información de la tarjeta y es devuelto a tus servidores para que puedas utilizarlo a través de Culqi Checkout, Culqi.JS o nuestras bibliotecas para móviles (iOS y Android). Este método nos asegura que ningún dato de tarjeta toque tus servidores y permite que la integración cumple con la normativa PCI DSS.

Si no deseas hacer uso de los métodos de tokenización disponibles, también podrías crear tokens utilizando la API y tu llave pública. Pero en este caso recuerda que tu empresa será la responsable de cualquier procedimiento requerido por la normativa PCI DSS, como por ejemplo el siguiente autocuestionario. A diferencia de Culqi Checkout, Culqi.JS y los SDKs para móviles, la información de tu cliente no sería enviada directamente a Culqi así que no podríamos determinar si manejas o guardas esta información con seguridad.

> Los tokens tienen un tiempo de expiración de 5 minutos por lo que no pueden ser guardados y utilizados más de una vez. Para hacerlos permanentes y generar cargos posteriores o recurrentes, debes crear un cliente y posteriormente crear una tarjeta.

| Operación        | Método | URL                                  |               |
|------------------|--------|--------------------------------------|---------------|
| Crear Token      | POST   | https://secure.culqi.com/v2/tokens   | Ver Detalle → |
| Consultar Token  | GET    | https://api.culqi.com/v2/tokens/{id} | Ver Detalle → |
| Listar Tokens    | GET    | https://api.culqi.com/v2/tokens      | Ver Detalle → |
| Actualizar Token | PATCH  | https://api.culqi.com/v2/tokens/{id} | Ver Detalle → |

## Objeto Token

```json
{
  "object": "token",
  "id": "tkn_live_0CjjdWhFpEAZlxlz",
  "type": "card",
  "email": "richard@piedpiper.com",
  "creation_date": 1476132639,
  "card_number": "442328******1214",
  "last_four": "1214",
  "active": true,
  "iin": {
    "object": "iin",
    "bin": "442328",
    "card_brand": "Visa",
    "card_type": "credito",
    "card_category": "platinum",
    "issuer": {
      "name": "Silicon Valley Bank",
      "country": "United States",
      "country_code": "US",
      "website": "https://www.svb.com",
      "phone_number": "+1.800.774.7390"
    },
    "installments_allowed": [
    2, 4, 8, 12, 24, 36
    ]
  },
  "client": {
    "ip": "72.229.28.185",
    "ip_country": "United States",
    "ip_country_code": "US",
    "browser": "Google Chrome 56.0.2924.87",
    "device_fingerprint": "6rITdVTYkWfOrss8",
    "device_type": "escritorio"
  },
  "metadata": {
    "dni":"5831543"
  }
}
```

**Atributos**

## Crear Token

## Consultar Token

## Listar Tokens

## Actualizar Token

