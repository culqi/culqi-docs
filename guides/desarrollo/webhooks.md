---
id: webhooks
title: Webhooks
---

Usa webhooks para recibir notificaciones de los eventos que suceden en tu cuenta Culqi

## ¿Qué es un webhook?

Un webhook es una notificación del servidor de Culqi hacia tu servidor web a través de un endpoint URL que permitirá notificarte acerca de los diferentes eventos relacionados a tu negocio.

Por ejemplo, un evento representará alguna acción que sucedió en tu cuenta, como por ejemplo, la creación de un cargo exitoso o la finalización de una suscripción. Cada uno de estos acontecimientos creará un objeto Evento, que contendrá toda la información relevante acerca de lo que ha sucedido, incluyendo el tipo de evento y la información asociada al mismo.

## ¿Por qué es importante utilizar webhooks?

Los webhooks te permitirán recibir información sobre eventos que no suceden directamente haciendo una petición a la API de Culqi, es decir, que suceden de manera asíncrona; por ejemplo, un cargo asociado a un plan de suscripción.

Puedes usar webhooks para:

- Actualizar la información de un comprador en tu base de datos.
- Enviar correos electrónicos a tus clientes cuando falle el cargo de la suscripción.
- Actualizar tu contabilidad cuando una transferencia es realizada.

## Pasos recomendados para utilizar Webhooks

### Paso 1: Crear la URL donde llegará el Webhook

Esta URL deberá ser una página dedicada en tu servidor web donde recibirás la información del evento que quieras controlar. Deberás crear la URL utilizando un lenguaje de programación del lado del servidor, por ejemplo PHP, JAVA, Python, etc.

```php
?php
  // Recuperar el cuerpo de la solicitud y analizarlo como JSON
  $input = file_get_contents("php://input");
  $event_json = json_decode($input);

  // Escribir el Webhook en mi archivo "log/log-webhooks.json" de ejemplo
  $myfile = fopen("log/log-webhooks.json", "w")
  or die("Imposible abrir el archivo.");
  fwrite($myfile, $input);

  //Respuesta a Culqi
  http_response_code(200);
  $array = array(
    "response" => "Webhook de Culqi recibido"
  );
  echo json_encode($array);));
?
```

Podrás configurar la cantidad de eventos que quieras utilizando una URL por cada evento o la misma para todos los eventos.

### Paso 2: Iniciando la configuración de tu Webhook

Para comenzar a usar webhooks, debes ir a la opción de Eventos ubicada en el menú del Panel Culqi y dirigirte a la sección Webhooks.


Al hacer clic en Añadir, se mostrará el siguiente formulario que te servirá para seleccionar el tipo de evento y agregar la URL donde recibirás los webhooks:

> ¡ Valida que estas ingresando la URL correcta !

### Paso 3: Recibiendo la notificación

La información del evento será enviada como JSON en el cuerpo de la petición.

```
{
  "object": "event",
  "id": "evt_test_bV7SE39fcGodMPn4",
  "type": "charge.creation.succeeded",
  "creation_date": 1507073604156,
  "data": "{\"object\":\"charge\",\"merchant\":null,
    \"id\":\"chr_test_WYfqyKDstbtAQc95\",\"creationDate\":1507073602000,
    \"originalAmount\":2000,\"refundedAmount\":0,\"actualAmount\":2000,
    \"installments\":0,\"installmentsAmount\":null,\"currency\":\"PEN\",
    \"email\":\"ruelas@gmail.com\",\"description\":null,
    \"source\":{\"object\":\"token\",\"id\":\"tkn_test_qHLQK7Hf331fRyeg\",
    \"type\":\"card\",\"creationDate\":1507073594000,\"email\":\"token@culqi.com\",
    \"cardNumber\":\"411111******1111\",\"lastFourDigits\":\"1111\",
    \"active\":true,\"iin\":{\"object\":\"iin\",\"value\":\"411111\",
    \"brand\":\"Visa\",\"cardType\":\"credito\",\"cardCategory\":\"Clásica\",
    \"issuer\":{\"name\":\"JPMORGAN CHASE BANK, N.A.\",\"country\":\"UNITED STATES\",
    \"countryCode\":\"US\",\"website\":null,\"phone\":null},\"installments\":[2,4,6,8,10,12]},
    \"tokenClient\":{\"ip\":\"181.66.148.66\",\"countryIp\":\"Perú\",\"countryCodeIp\":\"PE\",
    \"browser\":null,\"deviceFingerprint\":null,\"device\":\"Escritorio\"},\"metadata\":{}},
    \"outcome\":{\"type\":\"venta_exitosa\",\"code\":\"AUT0000\",\"declineCode\":null,
    \"message\":\"La operación de venta ha sido autorizada exitosamente\",
    \"userMessage\":\"Su compra ha sido exitosa.\"},\"fraudScore\":65.0,
    \"client\":{\"name\":\"Liz\",\"lastName\":\"Ruelas\",\"address\":\"av. lima 123\",
    \"city\":\"Lima\",\"country\":\"PE\",\"phone\":\"985289566\",
    \"object\":\"client\"},\"dispute\":false,\"capture\":true,\"referenceCode\":\"0wcGubibFc\",
    \"metadata\":{},\"fee\":89,\"feeDetails\":{\"fixed_fee\":{\"amount\":null,\"currency\":null,
    \"exchangeRate\":null,\"exchangeRateCurrencyCode\":null,\"commision\":null,\"total\":null},
    \"variable_fee\":{\"amount\":null,\"currency\":\"PEN\",\"exchangeRate\":null,
    \"exchangeRateCurrencyCode\":null,\"commision\":0.0375,\"total\":75}},\"feeTaxes\":14,
    \"transferAmount\":1911,\"paid\":false,\"statementDescriptor\":\"CULQI*\",
    \"transferId\":null,\"operations\":null,\"duplicated\":false}"
  }
```

### Paso 4: Respondiendo a un webhook (Opcional)

Para que Culqi pueda confirmar la recepción de un webhook, el endpoint URL que has creado deberá devolver un código de estado HTTP 2xx.

### Paso 5: Revisar el historial de eventos

Una vez configurados, al momento de que Culqi haga la petición hacia tu servidor, podrás ver en el historial de eventos toda la información acerca de ese hecho.

## Mejores prácticas

Antes de pasar a producción, asegúrate de haber probado que los WebHooks funcionen correctamente. Puedes hacer esto registrándolos en el Panel del Entorno de pruebas y además, puedes simular el envío de un WebHook a tu sistema usando cualquier cliente REST. Te recomendamos usar Postman para realizar estas pruebas.

