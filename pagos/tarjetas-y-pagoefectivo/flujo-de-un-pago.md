---
id: flujo-de-un-pago
title: Flujo de un pago
---

Con Culqi, puedes implementar una variedad de medios de pagos en tu sitio web o móvil, a través de una sola integración.

Hay que tomar en consideración el uso de la Culqi API en el diseño del flujo de pago personalizado. Así como también la implementación obligatoria de Webhooks para los medios de pagos asíncronos (PagoEfectivo), dichos pagos quedan a la espera de un pago por parte del cliente dentro de un tiempo determinado.

En la siguiente ilustración podrás ver el flujo de trabajo completo de multipago:

![flujo-pago](/img/pagos/flujo_pago.svg)

1. Desde tu servidor haces una petición a Culqi API para crear una orden.
2. Tu sitio web o aplicación móvil, muestra el formulario de captura. Usando Culqi Checkout
3. El cliente ingresa la información de su tarjeta.
4. La información de la tarjeta es enviada de forma segura a los servidores de Culqi para ser tokenizada.
5. Una vez que retornamos el token a tu sitio web o aplicación móvil, este debe ser enviado a tu servidor para ser procesado.
6. Desde tu servidor haces una petición a los servidores Culqi para crear un Cargo inmediato o recurrente.
7. Una vez recibida la respuesta de Culqi, debes de mostrarle al cliente el resultado.
8. Si es que el cliente seleccionó otro PagoEfectivo, recibirás el evento de la respuesta final de la orden (pagada o expirada) en tu sitio web mediante webhook .


----

## Flujo clásico para pagos con tarjeta

En un flujo de pago típico para pagos con tarjeta, tu integración recopila la información de la tarjeta y crea un token, luego la utiliza de inmediato para realizar una creación de cargo. Como no hay ninguna acción adicional que el cliente pueda tomar, y los pagos con tarjeta proporcionan una respuesta síncrona, podemos confirmar de inmediato si el pago se realizó correctamente y que los fondos están garantizados. Si es que decides usar solo tarjetas, no es necesario el uso de webhooks. Mientras que para el multipago es obligatorio (al menos para recibir los cambios de estado de la orden).

----

## Uso requerido de Webhooks

Para PagoEfectivo, la respuesta del pago es incierta hasta que el cliente decida pagar pueden haber pasado horas o días. Esta transición ocurre de forma asíncrona e incluso puede ocurrir después de que el cliente haya abandonado su sitio web. Por estas razones, es esencial que tu integración para multipagos se base en los webhooks para determinar cuándo se pagó la orden o si nunca fue pagada.

Los webhooks son herramientas que sirven para poder notificar a tu sitio de ciertos eventos. Para comenzar a usarlos, puedes revisar nuestra guía de Webhooks

----

## Cuotas

En el pago con tarjetas, las cuotas permiten poder pagar una compra en varias partes (esto incluye intereses que dependen del banco de la tarjeta). Ahora, en la creación de cargos es posible especificar si es que el cobro a la tarjeta debe realizarse en un número determinado de cuotas. Esta característica solo es aplicable cuando se intenta crear un cargo a un tarjeta de crédito.

Para usar esta característica, simplemente al momento de la creación de un cargo indica en el campo installments el número de cuotas en que será cobrada la venta. Previamente, en la creación de token debes verificar en la respuesta si la tarjeta acepta las cuotas.

----

## Sobre Autorización y Captura

En la creación de cargos, ahora es posible especificar si el cargo debe capturarse o no. No capturarlo, significa que el dinero permanece congelado en la cuenta del cliente. Un cargo puede autorizarse y ser capturado luego, pero el tiempo para poder capturarlo no debe exceder los 4 días desde la creación del cargo. Si en caso no llegarás a capturar un cargo, este expirará a los cuatro días y se le devolvería el dinero al cliente, tener esto en cuenta es muy importante para evitar inconvenientes.

Para usar esta característica, simplemente al momento de la creación de un cargo indica en el campo capture el valor como "false" para indicar que aún no debe capturarse

