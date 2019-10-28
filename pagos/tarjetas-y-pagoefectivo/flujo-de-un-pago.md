---
id: flujo-de-un-pago
title: Flujo de un pago
---

Con Culqi, implementar pagos en tu sitio web o móvil resulta ser simple. En la siguiente ilustración podrás ver el flujo de trabajo completo:

1. Tu sitio web o aplicación móvil, muestra el formulario de captura. Usando Culqi Checkout, Culqi JS o nuestras bibliotcas en iOS o Android

2. El cliente ingresa la información de su tarjeta.

3. La información de la tarjeta es enviada de forma segura a los servidores de Culqi para ser tokenizada.

4. Una vez que retornamos el token a tu sitio web o aplicación móvil, este debe ser enviado a tu servidor para ser procesado.

5. Desde tu servidor haces una petición a los servidores Culqi para crear un Cargo inmediato o recurrente.

6. Una vez recibida la respuesta de Culqi, debes de mostrarle al cliente el resultado.


## Pago en Cuotas

Las cuotas permiten poder pagar una compra en varias partes (esto incluye intereses que dependen del banco de la tarjeta). Ahora, en la creación de cargos es posible especificar si es que el cobro a la tarjeta debe realizarse en un número determinado de cuotas. Esta característica solo es aplicable cuando se intenta crear un cargo a un tarjeta de crédito.

Para usar esta característica, simplemente al momento de la creación de un cargo indica en el campo `installments` el número de cuotas en que será cobrada la venta. Previamente, en la creación de token debes verificar en la respuesta si la tarjeta acepta las cuotas.

Pasos para activar la opción de pago en cuotas:

1. Activar "installments : true" en Culqi.options
2. Crear el objeto Token con Culqi Checkout o Culqi JS.
3. Enviar "Culqi.token.metadata.installments" hacia tu servidor con Ajax.
4. Crear el cargo con el parámetro installments.

This is a link to [another document.](doc3.md)  
This is a link to an [external page.](http://www.example.com)


