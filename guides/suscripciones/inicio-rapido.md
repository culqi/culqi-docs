---
id: inicio-rapido
title: Inicio Rapido
---

El servicio "suscripciones" de Culqi, permiten a los comercios poder realizar cargos recurrentes de forma autómatica.

> Antes de comenzar con esta guía rápida, debes recordar tener a disposición tus llaves de comercio disponibles en el Panel Culqi en el entorno de Pruebas o Producción.

La guía rápida de suscripciones se ha divido en estos simples pasos:

1. Crear el objeto Plan
2. Crear el objeto token
3. Crear el objeto Cliente y objeto Tarjeta
4. Crear el objeto Suscripción

Después de crear un plan, solo necesitas seguir a partir del segundo paso para los siguientes clientes a subscribir. El resto de esta guía realiza los pasos a detalle, pero necesitas instalar en primera instancia las bibliotecas que dispone Culqi para correr los ejemplos de código que se encuentran en cada paso.

## Paso 1: Crear el objeto Plan

Los Planes son objetos que representan el costo, moneda, duración e intervalo de recurrencia. Tu puedes definir un solo plan o todos los que necesites de acuerdo a tu negocio (Plan Básico, Plan intermedio, Plan Premium, etc). Por ejemplo, si tienes dos tipos de clientes que usan el “Plan básico” para disfrutar de tu sitio web gratuitamente, pero hay otros clientes que usan características adicionales por 20 soles al mes (esto sería un Plan Premium), puedes crear el “Plan Básico” y “Plan Premium” en tu Panel Culqi. Aunque, también existe otra manera de crear planes y es vía API de Culqi o usando nuestras bibliotecas de lado del servidor, a continuación lo verás.

<!--DOCUSAURUS_CODE_TABS-->
<!--PHP-->
```php 
<?php
$culqi->Plans->create(
  array(
    "name" => "Plan de Prueba",
    "amount" => 2500,
    "currency_code" => "PEN",
    "interval" => "meses",
    "interval_count" => 1,
    "limit" => 12
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
plan.put("name", "plan-basico");
plan.put("trial_days", 15);
culqi.plan.create(plan);
```
<!--Python-->
```python
dir_plan = {
    'name': 'plan-test', 
    'amount': 1000,
    'currency_code': 'PEN', 
    'interval': 'dias', 
    'interval_count': 2,
    'limit': 10,'trial_days': 50
}

culqipy.Plan.create(dir_plan)
```
<!--Ruby-->
```ruby
Culqi::Plan.create(
      :name => "plan-test",
      :amount => 1000,
      :currency_code => "PEN",
      :interval => "dias",
      :interval_count => 2,
      :limit => 10,
      :trial_days => 5
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
  {"name", "plan-culqi"},
  {"trial_days", 15}
};

return new Plan(security).Create(map);
```
<!--END_DOCUSAURUS_CODE_TABS-->

> Nota: La llave privada nunca debe ser compartido o colocado de manera insegura en el código del cliente, solo debe estar presente en tu backend (código en el servidor).

## Paso 2: Crear el objeto Token

Culqi proporciona las siguientes formas de tokenizar la información de pago de tu cliente de manera segura bajo HTTPS, para integrarte debes elegir cualquiera de ellas:

- Culqi Checkout (Recomendada, la más simple)
- Culqi.js (Checkout personalizado)
- Biblioteca iOS
- Biblioteca Android

El proceso de tokenización asegura que la información sensible de la tarjeta no necesita tocar tu servidor y tu integración puede operar de forma compatible con la normativa PCI. Si en algún momento la información de la tarjeta llegara a pasar o es almacenada en tu servidor, tendrías que hacerte responsable por cualquiera de las pautas y/o auditorías de PCI DSS.

## Paso 3: Crear el objeto Cliente y objeto Tarjeta

Una vez creado un token, este debe ser enviado a tu servidor para realizar peticiones al API de Culqi (usando las bibliotecas clientes o directamente). En este paso debes realizar internamente dos acciones en tu servidor:

Creamos un objeto Cliente con algunos datos del comprador.
Con el Token (del paso 2) y el ID del customer se procede a crear una Tarjeta.

En el siguiente código puedes ver un ejemplo de las acciones:

<!--DOCUSAURUS_CODE_TABS-->
<!--PHP-->
```php 
<?php
//Crear Cliente
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
//Crear Tarjeta
$culqi->Cards->create(
  array(
    "customer_id" => "customer_id",
    "token_id" => "token_id"
  )
);
?>
```
<!--Java-->
```java
//Crear Cliente
Map customer = new HashMap();
customer.put("address","Av Lima 123");
customer.put("address_city","Lima");
customer.put("country_code","PE");
customer.put("email","tst"+new Util().ramdonString()+"@culqi.com");
customer.put("first_name","Test");
customer.put("last_name","Cuqli");
customer.put("phone_number",99004356);
culqi.customer.create(customer);

//Crear Tarjeta
Map card = new HashMap();
card.put("customer_id","{customer_id}");
card.put("token_id", "{token_id}");
culqi.card.create(card);
```
<!--Python-->
```python
# Crear Cliente
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

# Crear Tarjeta
dir_card = {
    'customer_id': 'customer_id', 
    'token_id': 'token_id'
}

culqipy.Card.create(dir_card)
```
<!--Ruby-->
```ruby
#Crear Cliente
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

#Crear Tarjeta
Culqi::Card.create(
      :customer_id => "customer_id",
      :token_id => "token_id"
)
```
<!--.Net-->
```csharp
#Crear Cliente
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

#Crear Tarjeta
Dictionary map = new Dictionary
{
  {"customer_id", "{customer_id}"},
  {"token_id", "{token_id}"}
};

return new Card(security).Create(map);
```
<!--END_DOCUSAURUS_CODE_TABS-->

## Paso 4: Crear el objeto Suscripción

Finalmente, crea la subscripción asociando el plan con la Tarjeta.

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

return new Subscription(security).Create(map);
```
<!--END_DOCUSAURUS_CODE_TABS-->

Perfecto, ahora tienes un cliente subscrito a un plan. Por detrás Culqi se encargará de crear los cargos correspondiente a futuro, según lo que hayas definido en el plan asociado.

¡Pero espera ahí, aún falta algo! Debido a que la actividad de las subscripciones ocurre automáticamente a partir de este punto, tu sistema necesita estar enterado de los cambios que puedan ocurrir con las subscripciones (fallos en los cargos, cargos exitosos, subscripciones canceladas, etc). Para ello debes visitar como controlar las subscripciones mediante webhooks (eventos que son notificados por Culqi y que debes escucharlos).