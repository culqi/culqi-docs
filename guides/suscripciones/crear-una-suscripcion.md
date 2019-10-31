---
id: crear-una-suscripcion
title: Crear una suscripción
---

La creación de suscripción comprende la parte final del flujo de una suscripción en Culqi, donde se crea la suscripción al comprador. Esta operación se realiza en el lado del servidor(backend) usando la API de Culqi.

> Este tutorial asume que ya has implementado una solución para capturar la información de las tarjetas de tus clientes, usando Culqi Checkout, Culqi.js o las bibliotecas de iOS y Android. Si aún no lo has hecho, revisa el inicio rápido de suscripciones, donde encontrarás paso a paso todo lo que necesitas para realizar una.

### Definir planes

Las suscripciones particularmente primero son configuradas bajo un plan. El plan define lo siguiente:

- Costo base del plan.
- Intervalo de cobro (cada semana, mes, año)
- Moneda (USD o PEN)

El plan define el comportamiento predeterminado para tus suscripciones y estos tres atributos son fijados al crear el plan. Los planes pueden ser creados vía API o desde el Panel de Culqi.

<!--DOCUSAURUS_CODE_TABS-->
<!--PHP-->
```php 
<?php
$culqi->Plans->create(
  array(
    "alias" => "plan-culqi".uniqid(),
    "amount" => 10000,
    "currency_code" => "PEN",
    "interval" => "months",
    "interval_count" => 1,
    "limit" => 12,
    "name" => "Plan de Prueba ".uniqid(),
    "trial_days" => 15
  )
);
?>
```
<!--Java-->
```java
Map plan = new HashMap();
Map metadata = new HashMap();
metadata.put("oder_id", "124");
plan.put("amount",1000);
plan.put("currency_code","PEN");
plan.put("interval","dias");
plan.put("interval_count",30);
plan.put("limit", 4);
plan.put("metadata", metadata);
plan.put("name", "plan-"+new Util().ramdonString());
plan.put("trial_days", 15);
culqi.plan.create(plan);
```
<!--Python-->
```python
dir_plan = {
    'name': 'plan-test-' + str(uuid.uuid1()), 
    'amount': 1000,
    'currency_code': 'PEN', 
    'interval': 'days', 'interval_count': 2,
    'limit': 10,
    'trial_days': 50
}

culqipy.Plan.create(dir_plan)

culqipy.Charge.create(dir_charge)
```
<!--Ruby-->
```ruby
Culqi::Plan.create(
      :name => "plan-test",
      :amount => 1000,
      :currency_code => "PEN",
      :interval => "day",
      :interval_count => 2,
      :limit => 10,
      :trial_days => 50
)
```
<!--.Net-->
```csharp
Dictionary map = new Dictionary
{
  {"amount", 10000},
  {"currency_code", "PEN"},
  {"interval", "dias"},
  {"interval_count", 15},
  {"limit", 2},
  {"metadata", metadata},
  {"name", "plan-culqi-"+GetRandomString()},
  {"trial_days", 15}
};

return new Plan(security).Create(map);
```
<!--END_DOCUSAURUS_CODE_TABS-->

!Eso es todo. Si la creación del plan es exitosa, puedes crear la suscripción a tu cliente


### Crear la suscripción


Luego de recibir el token de la tarjeta de tu cliente en tu servidor, debes usar nuestras bibliotecas o hacer una llamada directa al API para crear un objeto de tipo Cliente y otro de tipo Tarjeta que permite asociar el token con el Cliente. De esta manera, puedes usar la Tarjeta para realizar la suscripcón.

Una suscripción está compuesta principalmente por un `Plan`, un `Cliente` y una `Tarjeta`.

<!--DOCUSAURUS_CODE_TABS-->
<!--PHP-->
```php 
<?php
//Create Customer
$culqi->Customers->create(
  array(
    "address" => "av lima 123",
    "address_city" => "lima",
    "country_code" => "PE",
    "email" => "www@me.com",
    "first_name" => "Will",
    "last_name" => "Muro",
    "metadata" => array("test"=>"test"),
    "phone_number" => 899898999
  )
);

//Create Card
$culqi->Cards->create(
  array(
    "customer_id" => "customer_id",
    "token_id" => "token_id"
  )
);

//Create Subscription
$culqi->Subscriptions->create(
  array(
      "card_id" => "{card_id}",
      "plan_id" => "{plan_id}"
  )
);
?>
```
<!--Java-->
```java
//Create Customer
Map customer = new HashMap();
customer.put("address","Av Lima 123");
customer.put("address_city","Lima");
customer.put("country_code","PE");
customer.put("email","tst"+new Util().ramdonString()+"@culqi.com");
customer.put("first_name","Test");
customer.put("last_name","Cuqli");
customer.put("phone_number",99004356);
culqi.customer.create(customer);

//Create Card
Map card = new HashMap();
card.put("customer_id","{customer_id}");
card.put("token_id", "{token_id}");
culqi.card.create(card);

//Create Subscription
Map subscription = new HashMap();
subscription.put("card_id","{card_id}");
subscription.put("plan_id","{plan_id}");
culqi.subscription.create(subscription);
```
<!--Python-->
```python
#Create Customer
dir_refund = {
    'address': 'av lima 123', 
    'address_city': 'lima', 
    'country_code': 'PE', 
    'email': 'www@me.com',
    'first_name': 'Will', 
    'last_name': 'Muro', 
    'metadata': {'test':'test'}, 
    'phone_number': 899898999
}

culqipy.Customer.create(dir_refund)

#Create Card
dir_card = {
    'customer_id': 'customer_id', 
    'token_id': 'token_id'
}

culqipy.Card.create(dir_card)

#Create Subscription
dir_subscription = {
    "card_id": "card_id", 
    "plan_id": "plan_id"
}

culqipy.Subscription.create(dir_subscription)
```
<!--Ruby-->
```ruby
#Create Customer
Culqi::Customers.create(
    :address => "av lima 123",
    :address_city => "lima",
    :country_code => "PE",
    :email => "www@me.com",
    :first_name => "Will",
    :last_name => "Muro",
    :metadata => {:test => "test"},
    :phone_number => 899898999
)

#Create Card
Culqi::Card.create(
      :customer_id => "customer_id",
      :token_id => "token_id"
)

#Create Subscription
Culqi::Subscription.create(
      :card_id => "card_id",
      :plan_id => "plan_id"
)
```
<!--.Net-->
```csharp
//Create Customer
Dictionary map = new Dictionary
{
  {"address", "Av Lima 123"},
  {"address_city", "Lima"},
  {"country_code", "PE"},
  {"email", "test"+GetRandomString()+"@culqi.com"},
  {"first_name", "Test"},
  {"last_name", "Culqi"},
  {"phone_number", 99004356}
};

new Customer(security).Create(map);

//Create Card
Dictionary map = new Dictionary
{
  {"customer_id", "{customer_id}"},
  {"token_id", "{token_id}"}
};

return new Card(security).Create(map);

//Create Subscription
Dictionary map = new Dictionary
{
  {"card_id", "{card_id}" },
  {"plan_id", "{plan_id}" }
};

return new Subscription(security).Create(map);
```
<!--END_DOCUSAURUS_CODE_TABS-->


!Eso es todo¡ Si la creación de la suscripción es exitosa, la tarjeta y el cliente serán asociados a la suscripción y se realizará el cobor a la tarjeta de acuerdo al plan configurado. Por otro lado, si la suscripción falla, recibirás un `error` indicando el motivo.


### Añade información adicional a la Suscripción

Con Culqi puedes añadir Metadata a la mayoría de las peticiones de creación, y una de ellas es crear subscripción. Esto te permite añadir información adicional en la creación de la subscripción. Por ejemplo: Añadir información de edad, documento de identificación del cliente, etc.

Toda la metadata se encuentra visible en el Panel Culqi, al visualizar el detalle de una subscripción y también está disponible para ser exportado. Haciendo un pequeño ejemplo, esto permite que tu equipo de contabilidad o finanzas puede fácilmente conciliar las subscripciones en Culqi con las órdenes en tu sistema.

<!--DOCUSAURUS_CODE_TABS-->
<!--PHP-->
```php 
<?php
$culqi->Subscriptions->create(
  array(
      "card_id" => "{card_id}",
      "plan_id" => "{plan_id}"
  )
);
?>
```
<!--Java-->
```java
Map subscription = new HashMap();
subscription.put("card_id","{card_id}");
subscription.put("plan_id","{plan_id}");
culqi.subscription.create(subscription);
```
<!--Python-->
```python
dir_subscription = {
    "card_id": "card_id", 
    "plan_id": "plan_id"
}

culqipy.Subscription.create(dir_subscription)
```
<!--Ruby-->
```ruby
Culqi::Subscription.create(
      :card_id => "card_id",
      :plan_id => "plan_id"
)
```
<!--.Net-->
```csharp
Dictionary map = new Dictionary
{
  {"card_id", "{card_id}" },
  {"plan_id", "{plan_id}" }
};
```
<!--END_DOCUSAURUS_CODE_TABS-->

