---
id: flujo-de-una-suscripcion
title: Flujo de una suscripción
---

Con Culqi, implementar subscripciones en tu sitio web o móvil resulta ser simple y en la siguiente ilustración podrás ver el flujo de trabajo completo:

![flujo-pago](/devsite/img/pagos/flujo_pago.svg)

1. Tu sitio web o aplicación móvil, muestra el formulario de captura. Usando Culqi Checkout, Culqi JS o nuestras bibliotcas en iOS o Android
2. El cliente ingresa la información de su tarjeta.
3. La información de la tarjeta es enviada de forma segura a los servidores de Culqi para ser tokenizada.
4. Una vez que retornamos el token a tu sitio web o aplicación móvil, este debe ser enviado a tu servidor para ser procesado.
5. Desde tu servidor envías una petición a los servidores Culqi mediante el API para para crear un Cliente y luego una Tarjeta. Una vez creados exitosamente, puedes pasar a crear la Suscripción.
6. Muestra errores si es que existen en algún punto de las creaciones de objetos. De lo contratio muestra al comprador la creación exitosa de la suscripción.

## Estados de una suscripción

Una suscripción en Culqi solo puede tener dos estados durante su ciclo de vida. Cuando es recién creada, a partir del momento cero, posee un estado de '**active**' (activada), mientras que en el camino la suscripción puede ser cancelada por vía manual, por la API o automáticamente cuando sobrepasa los intentos de cargo fallidos consecutivos. Al ocurrir lo último, pasa a tener un estado de '**canceled**' (canceled).

## Uso de eventos para el control de una subscripción

El flujo de la suscripción no termina en la creación, es necesario controlar ciertas acciones que puedan ocurrir con la suscripción en el camino. Tales como: subscription.charge.succeded, subscription.charge.failed. Esto es importante, ya que sin esto no podrías tomar acciones con los usuarios en tu sistema.

Para controlar estos eventos usamos los webhooks que son herramientas que sirven para poder notificar a tu sitio de ciertos eventos, ve más sobre WebHooks y controlar los eventos especiales para suscripciones.

## Listado de eventos en Suscripciones

La siguiente lista menciona los tipos de `Eventos` que le corresponden a subscripciones: