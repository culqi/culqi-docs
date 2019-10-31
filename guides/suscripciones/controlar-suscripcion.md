---
id: controlar-una-suscripcion
title: Controlar una suscripción
---

Aprende cómo controlar las suscripciones existentes de tus clientes.

Los webhooks son herramientas que sirven para poder notificar a tu sitio de ciertos eventos. Los webhooks pueden ser usados con distintos propósitos, estos son importantes y requeridos cuando se usan suscripciones, donde la mayoria de actividad ocurre por cuenta de Culqi y asíncronamente.

Como las suscripciones son procesos automáticos, la mayoría de las actividades de las suscripciones es realizada por Culqi como: la creación de cargos, cancelación de suscripciones por pagos fallidos constantes. Para cada uno de esos eventos (y otros escenarios) debes tomar acciones mediante los webhooks que tenemos a disposición y tomar control de tus suscripciones. Para comenzar a usar los webhooks, debes ir al menú correspondiente en el Panel Culqi y configurarlos adecuadamente. Una vez configurados, el cuerpo del JSON que recibas a la URL asignada es uno similar a este:

En los usos más comunes de webhooks (eventos) con suscripciones se encuentran:

- Cuando una suscripción es cancelada
- El periodo de prueba de la suscripción termina
- El cargo de una suscripción es exitoso
- El cargo a una suscripción falla

Para que conozcas mejor acerca de WebHooks, revisa nuestra guía.

