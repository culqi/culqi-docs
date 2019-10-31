---
id: manejando-mi-cuenta
title: Manejando mi cuenta
---

Esta guía cubre algunos puntos bases acerca de cómo configurar y empezar a usar tu cuenta Culqi. Adicionalmente te daremos algunas recomendaciones de cómo mantener seguros los datos de tu cuenta.

A medida que empieces a integrar Culqi en tu página web o aplicación móvil, deberás empezar a trabajar con las siguientes funcionalidades y procesos:

- Entorno de producción y pruebas
- Llaves API
- Activación de tu cuenta
- Mantener tu cuenta segura

## Entornos de producción e integración

Cada cuenta creada en Culqi está dividida en dos ambientes: uno para realizar transacciones de pruebas y otro para realizar transacciones con tarjetas reales. Todas las peticiones al API estarán direccionadas a uno de los dos entornos y dependerá del tipo de llave que estés utilizado.

Los entornos de producción y pruebas fueron diseñados para que funcionen exactamente igual pero solo con unas pequeñas diferencias:

En el entorno de pruebas, las transacciones no son procesadas por las redes de bancos y solo se pueden utilizar tarjetas de prueba.
Podrás encontrar una guía paso a paso de cómo realizar tus pruebas en el entorno de integración utilizando nuestro producto de Pagos o Suscripciones.

## Llaves

Cada uno de los entornos de tu cuenta (producción y pruebas), funcionan únicamente con un juego de llaves que han sido creadas exclusivamente para cada entorno y de esta manera Culqi se da cuenta con qué ambiente quieres interactuar. Las llaves correspondientes a tu cuenta las podrás encontrar en el Panel de Culqi.

Para conseguir las API Keys del entorno que deseas utilizar, deberás ingresar al Panel del entorno y dirigirte a la sección Desarrollo – API Keys:



Adicionalmente a las API Keys relacionados a los entornos de integración y producción, existen dos tipos de API Keys: públicas y secretas.

Las llaves públicas están diseñadas solamente para identificar tu cuenta con Culqi y no son un secreto. En otras palabras, pueden ser publicadas de manera segura en tu Culqi.JS, código JavaScript o en tu aplicación Android o iOS. Las llaves públicas solo tiene el poder de crear tokens.
En cambio, las llaves privadas deben ser guardadas en tus servidores de manera confidencial. Este tipo de llaves pueden realizar cualquier petición al API de Culqi sin ninguna restricción.
En caso quieras renovar ambas llaves de manera inmediata puedes realizarlo a través del Panel y estás quedarán desactivadas en el momento. Cuando tu juego de llaves quedan desactivadas, ya no podrás volver a utilizarlos nuevamente.


## Activación de tu cuenta

Antes de activar tu cuenta, solo podrás interactuar con el entorno de pruebas de Culqi. No te preocupes. Todas las funcionalidades del entorno de producción están también disponibles también en el entorno de pruebas, la única diferencia es que no se pueden crear transacciones con tarjetas reales hasta que actives tu cuenta.

Para activar tu cuenta en Culqi solo debes completar un pequeño formulario que contiene información de tu negocio, datos de tu cuenta(s) bancaria(s) y el tipo de relación que tienes con el negocio. Adicionalmente necesitamos que adjuntes los siguientes documentos escaneados:

Tu Ficha RUC como persona natural o persona jurídica que la puedes descargar en Sunat En Línea.
Un Estado de Cuenta Bancaria escaneado en la moneda en la que quieres recibir los cargo. Esto nos permite saber en qué moneda y dónde depositarte las ventas.
Una vez que recibimos la información de tu negocio, revisamos que todo lo enviado cumpla con nuestros términos de servicio y en aproximadamente 6 días hábiles te enviamos los accesos al entorno de producción. Si vemos que pueda presentarse algún problema, nos comunicaremos contigo para tratar de resolverlo inmediatamente.

## Mantener tu cuenta segura

Una vez que ya está configurada tu cuenta en el modo de producción de Culqi, es importante que mantengas todos los datos confidenciales y seguros. Aquí algunas recomendaciones:

Mantén segura toda la información privada. Tu contraseña y llave privadas deben ser tratadas con la mayor confidencialidad posible.
No reutilices la contraseña de Culqi. Tu contraseña debe ser única para Culqi. Si utilizas una contraseña de Culqi en otro sitio y este sitio es hackeado, un atacante podría utilizar estas credenciales para ingresar a u cuenta en Culqi.
Actualiza tu computadora y browser de manera regular. Recomendamos configurar las descargas de los sistemas operativos de manera automática ( OS X, Windows). Esto te permitirá automatizar a tu sistema para que reaccione de manera automática a ataques y malwares.
Cuidado con phishing. Todos los sitios genuinos de Culqi utilizan el dominio culqi.com y son HTTPS. Si recibes un email que no esperas, anda directamente a nuestro sitio e inicia sesión. No ingreses tu contraseña después de clickear un link en un correo.
Una vez que ya está configurada tu cuenta en el modo de producción de Culqi, es importante que mantengas todos los datos confidenciales y seguros. Aquí algunas recomendaciones:

- **Mantén segura toda la información privada.** Tu contraseña y llave privadas deben ser tratadas con la mayor confidencialidad posible.
- **No reutilices la contraseña de Culqi.** Tu contraseña debe ser única para Culqi. Si utilizas una contraseña de Culqi en otro sitio y este sitio es hackeado, un atacante podría utilizar estas credenciales para ingresar a u cuenta en Culqi.
- **Actualiza tu computadora y browser de manera regular.** Recomendamos configurar las descargas de los sistemas operativos de manera automática ( OS X, Windows). Esto te permitirá automatizar a tu sistema para que reaccione de manera automática a ataques y malwares.
- **Cuidado con phishing.** Todos los sitios genuinos de Culqi utilizan el dominio culqi.com y son HTTPS. Si recibes un email que no esperas, anda directamente a nuestro sitio e inicia sesión. No ingreses tu contraseña después de clickear un link en un correo.

