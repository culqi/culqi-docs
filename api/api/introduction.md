---
id: introduction
title: Introducción
---

La API de Culqi está construido bajo los estándares de REST. Es decir, nuestra API posee URLs orientada a recursos, y hace uso de los códigos de respuesta HTTP para indicar los posibles errores en el API. Es importante indicar que se encuentra implementada una autenticación HTTP (Bearer Token), solicitada en cada petición. Además, soportamos las solicitudes HTTP de origen cruzado (CORS), permitiendo que tu sitio y Culqi puedan interactuar de manera segura mediante nuestra API desde una aplicación cliente (aunque NUNCA deberías exponer tu Llave Secreta en el código de la aplicación web cliente). Por otro lado, un objeto JSON es retornado en cada una las peticiones hacia el API, incluyendo los errores. Adicionalmente nuestras bibliotecas convierten las respuestas en objetos específicos para cada lenguaje soportado.

Finalmente, para que puedas comenzar a experimentar con nuestra API, todas las cuentas registradas en Culqi poseen llaves para el entorno de pruebas (Regístrate y obtén tus llaves) y el entorno de producción. Usando las llaves de prueba las transacciones nunca pasan por las redes bancarias y no tienen ningún costo. (¡Recuerda usar tarjetas de prueba, no tarjetas reales al probar!).

## Recursos Principales

### [/tokens](#)

Captura de manera segura datos sensibles de tarjetas de crédito y débito directamente desde el navegador de tu cliente sin que toquen tus servidores.

### /charges

Realiza un cargo o cobro a las tarjetas de crédito o débito de tus clientes usando un token de cargo único o una tarjeta que haya sido guardada previamente. 

### /plans

Define un plan que contenga información acerca del tipo de cargo, frecuencia, intervalo y monto que quieres cobrarle a un cliente de manera recurrente. 

### /subscriptions

Realiza cargos recurrentes a tus clientes asociando una tarjeta de crédito o débito guardada con un plan de suscripción creado previamente. 

### /customers

Crear un cliente te permitirá asociar un usuario a una o varias tarjetas de crédito o débito para realizarles cargos recurrentes o pagos en un solo click. 

### /cards

Guarda información de las tarjetas de tus clientes frecuentes para realizarles cargos sin que vuelvan a colocar toda la información de sus tarjetas de crédito o débito. 