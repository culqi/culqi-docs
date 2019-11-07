---
id: seguridad
title: Guía de Seguridad en tu Integración
sidebar_label: Seguridad
---

En este artículo aprenderás cómo asegurar el correcto uso de la norma PCI DSS Compliance y las mejoras prácticas de comunicación entre tus clientes y servidores.

Cualquier entidad involucrada con la captura, envío y almacenamiento de datos de tarjetas de crédito y débito debe cumplir con la normativa PCI DSS Payment Card Industry Data Security Standards. Culqi te simplifica este proceso ofreciéndote una integración 100% segura y te ayuda a cumplir con los siguientes estándares:

Nos aseguramos que la página de captura de tarjetas de crédito y débito utilizadas por tus clientes tengan un certificado TLS (Transport Layer Security) para que puedan hacer uso del estandar HTTPS.
Para capturar datos sensibles de tarjetas de manera segura hemos creado Culqi Checkout y Culqi.JS. De esta manera nos aseguramos que la data es transmitida directamente a los servidores de Culqi sin la necesidad que pasen por los tuyos.

## Usar TLS

Usar TLS significa asegurar la transmisión de datos entre tu cliente (la aplicación o browser que está utilizando) y tus servidores. Originalmente fue realizado utilizando el protocolo SSL (Secure Sockets Layer), sin embargo este procedimiento fue remplazado por el estándar TLS ya que se considera desactualizado y poco seguro. Sin embargo, el término ¨SSL¨ continúa siendo utilizado de manera coloquial cuando se refiere a TLS y su función de proteger y transmitir datos seguros.

Las páginas de captura de datos sensibles de tarjeta deben utilizar obligatoriamente un certificado SSL ya que reduce significativamente el riesgo a tu negocio y a tus clientes de estar expuesto a ataques de intermediarios protegiéndote de la siguiente manera:

Encriptar y verificar la integridad del tráfico entre el cliente y tu servidor
Verificar que el cliente está comunicándose desde un servidor correcto. En la práctica, esto usualmente significa que el dueño del servidor y el dueño del dominio son la misma entidad. Esto ayuda a prevenir ataques de hombre en el medio. Sin él, no habría garantía que estás encriptando el tráfico de manera adecuada.
Tus clientes se sienten más cómodos al compartir los datos de sus tarjetas de crédito y débito en páginas que visiblemente son HTTPS y te puede ayudar a aumentar tu tasa de conversión.

TLS se requiere solamente en el entorno de producción y puedes empezar a realizar tus pruebas sin tener configurado el certificado. Una vez que estés listo para aceptar cargos de verdad, necesitas tener sí o si configurado el certificado.

## Configurar TLS

Para utilizar TLS es necesario configurar un certificado digital, emitido por una proveedor oficial certificado (CA). Una vez que el certificado está configurado, este nos asegurará que el cliente está comunicándose con el servidor correcto y no con un impostor. Es importante que escojas un proveedor digital certificado y de muy buena reputación como:

- Lets Encrypt
- DigiCert
- NameCheap

Los certificados podrían variar de costo y dependerán del tipo de certificado y el proveedor de tu preferencia. Por ejemplo, Lets Encryp es una autoridad certificadora que provee certificados digitales gratuitos.

Conceptualmente, configurar un certificado TLS es algo bastante sencillo: se compra el certificado y luego hay que configurar tu servidor para poder utilizarlo. Sin embargo, el proceso real puede ser algo fastidioso por lo que recomendamos que sigas la guía de instalación oficial de tu proveedor.

Te recomendamos que utilices el SSL Server Test desarrollado por Qualys Lab para asegurarte de que has realizado la configuración del certificado de manera segura.

## Cumplimiento PCI DSS

Todos los clientes de Culqi deben cumplir con la norma PCI Data Security Standars (PCI DSS). Para ello, Culqi Checkout y Culqi.JS cumplen con todos requerimientos y políticas de seguridad correspondiente al auto-cuestionario A SAQ A o Self-Assessment Questionnaire, realizando todas las transmisiones de data sensible del cliente a través de un IFRAME que carga desde el dominio culqi.com y que es controlado por Culqi.

¡ Para asegurar que tu integración está utilizando la versión más actualizado de Culqi Checkout y Culqi.JS, ambos deberán ser cargados directamente desde el dominio de Culqi y no deben estar alojados localmente!

Por ende, mientas que cargues tu página web con TLS y utilices Culqi Checkout o Culqi.JS como la única forma de manejar información sensible de tarjetas, Culqi automáticamente pre-completará este auto-cuestionario por ti y no necesitarás pasar por la auditoría PCI. Si los datos sensibles de la tarjeta son trasmitidos hacia tus servidores, eres responsable de seguir las directrices de la norma PCI DSS por manejar información de tarjetas y periódicamente serás auditado por un auditor certificado por la norma PCI.

Dependiendo de cómo utilices Culqi, puede ser que te hagamos algunas preguntas adicionales sobre cómo manejas datos sensibles de tarjetas una vez que empieces a aceptar pagos online.

### Información no sensible

Culqi devuelve en cada respuesta del cargo información no sensible de la tarjeta. Esto incluye el tipo de tarjeta utilizada, los cuatro últimos números de la tarjeta y la fecha de expiración. Esta información no está sujeta a la norma PCI, por lo que serás capaz de guardar cualquiera de estos parámetros en tu base de datos.

### Algunas consideraciones adicionales

Incluir JavaScript de otros sitios en tus desarrollos podría llegar a ser un riesgo de seguridad, ya que tu seguridad también dependerá de un tercero. Si en algún momento estos sitios son atacados, puede ser que el atacante ejecute un código arbitario en tu sitio web. En la práctica, muchos sitios utilizan JavaScript para servicios como Google Analytics. Sin embargo, es algo que debes estar al tanto y tratar de minimizar.

Si estás haciendo uso de Eventos recomendamos utilizar TLS para el Endpoint. De esta manera evitaremos que el tráfico sea interceptado y las notificaciones alteradas.

Finalmente, cumplir con PCI DSS no es el único reto en seguridad. Debería ser una prioridad en tu negocio y llegar a implementar un sistema seguro a través de las recomendaciones de:

- OWASP
- SANS
- NIST