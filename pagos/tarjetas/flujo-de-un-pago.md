---
id: flujo-de-un-pago
title: Flujo de un pago
---

Con Culqi, implementar pagos en tu sitio web o móvil resulta ser simple. En la siguiente ilustración podrás ver el flujo de trabajo completo:

![flujo-pago](/img/pagos/flujo_pago.svg)

1. Tu sitio web o aplicación móvil, muestra el formulario de captura. Usando Culqi Checkout, Culqi JS o nuestras bibliotcas en iOS o Android
2. El cliente ingresa la información de su tarjeta.
3. La información de la tarjeta es enviada de forma segura a los servidores de Culqi para ser tokenizada.
4. Una vez que retornamos el token a tu sitio web o aplicación móvil, este debe ser enviado a tu servidor para ser procesado.
5. Desde tu servidor haces una petición a los servidores Culqi para crear un Cargo inmediato o recurrente.
6. Una vez recibida la respuesta de Culqi, debes de mostrarle al cliente el resultado.

----

## Pago en Cuotas

Las cuotas permiten poder pagar una compra en varias partes (esto incluye intereses que dependen del banco de la tarjeta). Ahora, en la creación de cargos es posible especificar si es que el cobro a la tarjeta debe realizarse en un número determinado de cuotas. Esta característica solo es aplicable cuando se intenta crear un cargo a un tarjeta de crédito.

Para usar esta característica, simplemente al momento de la creación de un cargo indica en el campo `installments` el número de cuotas en que será cobrada la venta. Previamente, en la creación de token debes verificar en la respuesta si la tarjeta acepta las cuotas.

Pasos para activar la opción de pago en cuotas:

1. Activar "installments : true" en Culqi.options
2. Crear el objeto Token con Culqi Checkout o Culqi JS.
3. Enviar "Culqi.token.metadata.installments" hacia tu servidor con Ajax.
4. Crear el cargo con el parámetro installments.

En este enlace podras encontrar los pasos adicionales para tener la opción de pago en cuotas (ejemplo con PHP).

----

## Sobre Autorización y Captura

En la creación de cargos, ahora es posible especificar si el cargo debe capturarse o no. No capturarlo, significa que el dinero permanece congelado en la cuenta del cliente. Un cargo puede autorizarse y ser capturado luego, pero el tiempo para poder capturarlo no debe exceder los **4 días desde la creación del cargo**. Si en caso no llegarás a capturar un cargo, este expirará a los cuatro días y se le devolvería el dinero al cliente, tener esto en cuenta es muy importante para evitar inconvenientes.

Para usar esta característica, simplemente al momento de la creación de un cargo indica en el campo `capture` el valor como "false" para indicar que aún no debe capturarse

Pasos para realizar la autorización y captura de un cargo:

1. Crear el objeto Token con Culqi Checkout o Culqi JS.
2. Crear el objeto Cargo con el parámetro "capture:false"
3. Capturar el cargo con el API de captura de cargo.

----

## Eventos en cargos

Los webhooks son herramientas que sirven para poder notificar a tu sitio de ciertos eventos. Para comenzar a usarlos, puedes revisar nuestra guía de WebHooks

### Listado de eventos en cargos

La siguiente lista menciona los tipos de Eventos que le corresponden a cargos:

| Evento          | Descripción                                              |
|-----------------|----------------------------------------------------------|
| charge.captured | Un cargo es capturado                                    |
| charge.failed   | Un cargo ha fallado.                                     |
| charge.refunded | Un cargo ha sido devuelto.                               |
| charge.succeed  | Un cargo fue exitoso.                                    |
| charge.expired  | Un cargo expiró, debido a que no fue capturado a tiempo. |
| charge.updated  | Un cargo se actualizó.                                   |