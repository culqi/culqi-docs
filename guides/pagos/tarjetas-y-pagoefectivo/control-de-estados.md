---
id: control-de-estados
title: Control de estados
---

## Eventos de orden

Para culminar la integración de Culqi con tarjetas y PagoEfectivo es necesaria la sincronización con tu sistema. Tu sitio web necesita los cambios de estado de las órdenes, en caso de cambiar a pagadas, expiradas o algún otro estado. Esto es posible mediante el uso de webhooks, si tienes dudas acerca sobre los webhooks puedes entrar aquí.

En este caso de PagoEfectivo, es obligatorio implementar un Webhook en el lado del servidor (backend) para recibir el evento de Cambio de estado de orden ('order.status.changed').

Seguiremos los siguientes pasos para implementar el webhook (endpoint) que recibirá el evento:

- Creación de Webhook para recepción de eventos
- Configuraciones en Culqi Panel

### Creación de Webhook para recepción de eventos

Para comenzar a usar webhooks, se debe crear un endpoint el cual soporte la recepciòn del evento que Culqi enviará. Este es un ejemplo en PHP que puede servirte de base para implementar el tuyo en tu backend.

Lo que recibirá el endpoint es un evento en forma de petición POST conteniendo en el cuerpo un objeto JSON. Este punto se detalla mucho mejor en la sección de Webhooks.

### Configuraciones en Culqi Panel

Luego, como siguiente paso, debes ir a la opción de Eventos ubicada en el menú del Panel Culqi y dirigirte a la sección Webhooks.

En primer lugar, debemos crear un Webhook del tipo ‘order.status.changed’, esto nos servirá para comenzar a recibir los eventos de cambio de estado en la URL indicada. Es opcional activar autenticación básica.


Finalmente, registramos el webhook. Recordar que la URL a ingresar debe ser un endpoint público de tu Webhook creado, además, habilitado para peticiones POST.


## Pruebas

A medida que empieces a integrar Culqi en tu página web o aplicación móvil, deberás empezar a trabajar con las siguientes funcionalidades y procesos para realizar tus pruebas antes de salir a producción:

### Entorno de pruebas

Aquí puedes realizar todas las pruebas necesarias para que tu integración sea exitosa. En este entorno no se realizan operaciones reales, por lo que te permite probar todas los casos de respuesta en Culqi. Puedes conocer más acerca de este entorno, aquí.


### Testeando recepción de eventos

Para tus pruebas en ambiente de integración se adjuntan ejemplos de tramas JSON que se envían al webhook, todas llegan con el tipo de evento ‘order.status.changed’, con ello puedes simular los eventos para el cambio de estado:

- Cuando la orden es pagada (ver ejemplo)
- Cuando la orden llega a expirar (ver ejemplo)
- Cuando la orden es eliminada (ver ejemplo)

Por otro lado, para las pruebas de funcionamiento en producción de los eventos puedes realizar los siguientes casos:

- Crear una orden con 5 minutos de expiración, de esta manera recibirás el evento de cambio de estado con una orden expirada.
- Crear órdenes con distintos tiempos de expiración y esperar los eventos pasados sus tiempos de expiraciòn.

> Estas últimas pruebas se pueden efectuar en producción debido que en este ambiente está habilitado el envío de eventos automáticos. Por otro lado, en integración se pueden simular estos eventos con Postman de manera manual, arriba se adjuntan los ejemplos de peticiones JSON que enviamos hacia el webhook.

### Recomendaciones

Mientras te integras y realizas pruebas, podrás ver todos las peticiones que hagas a la API en el Panel de Culqi. Mira el registro de tus peticiones aquí. Además, te dejamos estas recomendaciones a considerar:

- Se recomienda probar utilizando la herramienta Postman la funcionalidad del Webhook, simulando peticiones POST hacia el endpoint público. Se adjuntan las tramas JSON ejemplo para que puedas realizar estas pruebas.

- En caso de requerir mayor seguridad, puedes usar la autenticación básica al momento de crear el webhook en el Panel Culqi. En tu endpoint también debes solicitar esta autenticación básica.

- Es importante salir a producción SOLO si el webhook (endpoint receptor) de cambio de estado de la orden está implementado correctamente y con las acciones que debes realizar en tu sistema, considerando todos los casos de cambio (Expirada, Pagada y Eliminada).