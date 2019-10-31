---
id: cancelar-una-suscripcion
title: Cancelar una suscripción
---

Aprende cómo cancelar las suscripciones y dejar de cobrar a tus clientes de forma automática.

Por defecto, una suscripción continúa, y se sigue cobrando al cliente hasta que sea cancelada.

Las suscripciones automáticamente son canceladas cuando sobrepasan el límite de reintentos de cargos fallidos. Pero también puedes cancelar suscripciones cuando lo decidas, ya sea a través de la API o del Panel Culqi

Pueden haber distintas razones en las que debas cancelar una suscripción, como por ejemplo:

- El cliente decide solicitar la cancelación de su suscripción.
- El cliente decide darse de baja de tu sitio.
- Y otras razones...

## Cancelando una suscripción

Las suscripciones pueden ser canceladas a través de la API usando las bibliotecas clientes de Culqi o canceladas vía el Panel de Culqi.

Por defecto, la cancelación de un suscripción es inmediata. Una vez que una suscripción es cancelada, no se generarán más cargos en el futuro para el cliente. Esta acción es irreversible.


<!--DOCUSAURUS_CODE_TABS-->
<!--PHP-->
```php 
<?php
    $culqi->Subscriptions->delete("id");
?>
```
<!--Java-->
```java
culqi.subscription.delete("id");
```
<!--Python-->
```python
culqipy.Subscription.delete("id");
```
<!--Ruby-->
```ruby
Culqi::Subscription.delete("id")
```
<!--.Net-->
```csharp
return new Subscription(security).Delete("id");
```
<!--END_DOCUSAURUS_CODE_TABS-->

