---
id: cargos
title: Cargos
---

## Crear cargo

La creación de un cargo comprende el paso final del flujo de un pago en Culqi, en este paso se efectúa el cobro a la tarjeta del cliente. A diferencia de la captura de la información que ocurre en el navegador o aplicación móvil del cliente, la creación de un cargo debe ser realizada en el lado del servidor(backend) usando la API de Culqi.

> Este tutorial asume que ya has implementado una solución para capturar la información de las tarjetas de tus clientes, usando Culqi Checkout, Culqi.js o las bibliotecas de iOS y Android. Si aún no lo has hecho, revisa el inicio rápido de pagos, donde encontrarás paso a paso todo lo que necesitas para realizar un cargo.

En Culqi tienes dos opciones al momento de querer realizar un cargo, puedes:

- Crear un cargo único
- Crear cargos posteriores(One Click Payments)

Adicionalmente, en ambos casos, puedes:

- Añadir información adicional al cargo

----

### Crear un cargo único

Luego de recibir el token de la tarjeta de tu cliente en tu servidor, debes usar nuestras bibliotecas o hacer una llamada directa al API para crear el cargo.

> Recuerda que un token expira a los 5 minutos de haber sido creado y puede ser usado una sola vez.


<!--DOCUSAURUS_CODE_TABS-->
<!--PHP-->
```php 
<?php
$culqi->Charges->create(
 array(
     "amount" => 1000,
     "currency_code" => "PEN",
     "email" => "test@culqi.com",
     "source_id" => "toke_id or card_id"
   )
);
?>
```
<!--Java-->
```java
Map charge = new HashMap();
Map metadata = new HashMap();
metadata.put("oder_id", "124");
charge.put("amount",1000);
charge.put("currency_code","PEN");
charge.put("email","test@culqi.com");
charge.put("source_id", token().get("id").toString());
culqi.charge.create(charge);
```
<!--Python-->
```python
dir_charge = {
    'amount': 1000, 
    "capture": True, 
    'country_code': 'PE',
    'description': 'Venta de prueba', 
    'email': 'wmuro@me.com',
    'installments':0,
    "source_id": "toke_id or card_id"
}

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
Dictionary metadata = new Dictionary
{
  {"order_id", "777"}
};

Dictionary map = new Dictionary
{
  {"amount", 1000},
  {"capture", true},
  {"currency_code", "PEN"},
  {"description", "Venta de prueba"},
  {"email", "wmuro@me.com"},
  {"installments", 0},
  {"metadata", metadata},
  {"source_id", (string)json_object["id"]}
};

new Charge(security).Create(map);
```
<!--END_DOCUSAURUS_CODE_TABS-->

¡Eso es todo! Si la creación del cargo es exitosa, se realizará el cobro a la tarjeta y recibirás el dinero en tu cuenta después de 5 días. Por otro lado, si el cargo falla, recibirás un error indicando el motivo.

----

### Crear cargos posteriores

Si bien los tokens de Culqi solo pueden ser usados una vez y expiran a los 5 minutos, no significa que por cada compra que haga debes solicitarle a tu cliente la información de su tarjeta. Culqi te proporciona la oportunidad de crear un objeto de tipo Cliente y otro de tipo Tarjeta que permite asociar el token con el Cliente. De esta manera, puedes usar la Tarjeta para realizar el cargo en cualquier momento.

> Al momento de realizar la creación de la Tarjeta, se realizará un cargo de un sol a la tarjeta del cliente para validar que esta exista.


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

//Crear Cargo
$culqi->Charges->create(
 array(
     "amount" => 1000,
     "capture" => true,
     "currency_code" => "PEN",
     "description" => "Venta de prueba",
     "email" => "test@culqi.com",
     "installments" => 0,
     "source_id" => "toke_id or card_id"
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

//Crear Cargo
Map charge = new HashMap();
Map metadata = new HashMap();
metadata.put("oder_id", "124");
charge.put("amount",1000);
charge.put("capture", true);
charge.put("currency_code","PEN");
charge.put("description","Venta de prueba");
charge.put("email","test@culqi.com");
charge.put("installments", 0);
charge.put("metadata", metadata);
charge.put("source_id", token().get("id").toString());
culqi.charge.create(charge);
```
<!--Python-->
```python
//Crear Cliente
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

//Crear Tarjeta
dir_card = {
    'customer_id': 'customer_id', 
    'token_id': 'token_id'
}

culqipy.Card.create(dir_card)

//Crear Cargo
dir_charge = {
    'amount': 1000, 
    "capture": True, 
    'country_code': 'PE',
    'description': 'Venta de prueba', 
    'email': 'wmuro@me.com',
    'installments':0, 
    "source_id": "toke_id or card_id"
}

culqipy.Charge.create(dir_charge)
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

#Crear Cargo
Culqi::Charge.create(
      :amount => 1000,
      :capture => true,
      :currency_code => "PEN",
      :description => "Venta de prueba",
      :installments => 0,
      :metadata => "",
      :source_id => "toke_id or card_id"
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

#Crear Cargo
Dictionary metadata = new Dictionary
{
  {"order_id", "777"}
};

Dictionary map = new Dictionary
{
  {"amount", 1000},
  {"capture", true},
  {"currency_code", "PEN"},
  {"description", "Venta de prueba"},
  {"email", "wmuro@me.com"},
  {"installments", 0},
  {"metadata", metadata},
  {"source_id", (string)json_object["id"]}
};

new Charge(security).Create(map);
```
<!--END_DOCUSAURUS_CODE_TABS-->

!Eso es todo! Ahora ya puedes realizar cargos posteriores a la tarjeta de tu cliente. Debes de asegurarte de guardar el ID de la tarjeta en tu servidor y relacionarlo con la cuenta de tu cliente. Para cualquier compra futura, solo debes de enviar el Id de la Tarjeta al momento de crear el cargo.

----

### Añadir información adicional al cargo

Con Culqi puedes añadir metadata a la mayoría de las peticiones de creación, y una de ellas es crear cargos. La información en este campo es para tu uso interno. Puedes añadir, por ejemplo: Información de edad, documento de identificación del cliente, etc.

Toda la metadata se encuentra visible en el Panel Culqi, al visualizar el detalle de un cargo y está disponible para ser exportada. Haciendo un pequeño ejemplo, esto permite que tu equipo de contabilidad o finanzas puede fácilmente conciliar los cargos en Culqi con las órdenes en tu sistema:


<!--DOCUSAURUS_CODE_TABS-->
<!--PHP-->
```php 
<?php
$culqi->Charges->create(
 array(
     "amount" => 1000,
     "capture" => true,
     "currency_code" => "PEN",
     "description" => "Venta de prueba",
     "email" => "test@culqi.com",
     "installments" => 0,
     "source_id" => "toke_id or card_id"
   )
);
?>
```
<!--Java-->
```java
Map charge = new HashMap();
Map metadata = new HashMap();
metadata.put("oder_id", "124");
charge.put("amount",1000);
charge.put("capture", true);
charge.put("currency_code","PEN");
charge.put("description","Venta de prueba");
charge.put("email","test@culqi.com");
charge.put("installments", 0);
charge.put("metadata", metadata);
charge.put("source_id", token().get("id").toString());
culqi.charge.create(charge);
```
<!--Python-->
```python
dir_charge = {
    'amount': 1000, 
    "capture": True, 
    'country_code': 'PE',
    'description': 'Venta de prueba', 
    'email': 'wmuro@me.com',
    'installments':0, 
    "source_id": "toke_id or card_id"
}

culqipy.Charge.create(dir_charge)
```
<!--Ruby-->
```ruby
Culqi::Charge.create(
  :amount => 1000,
  :capture => true,
  :currency_code => "PEN",
  :description => "Venta de prueba",
  :installments => 0,
  :metadata => "{"dni":"12345678"}",
  :source_id => "token_id or card_id"
)
```
<!--.Net-->
```csharp
Dictionary metadata = new Dictionary
{
  {"order_id", "777"}
};

Dictionary map = new Dictionary
{
  {"amount", 1000},
  {"capture", true},
  {"currency_code", "PEN"},
  {"description", "Venta de prueba"},
  {"email", "wmuro@me.com"},
  {"installments", 0},
  {"metadata", metadata},
  {"source_id", (string)json_object["id"]}
};

new Charge(security).Create(map);
```
<!--END_DOCUSAURUS_CODE_TABS-->

## Tipos de Denegaciones

Los cargos a una tarjeta pueden fallar por una variedad de razones y es frustrante cuando representan la pérdida legítima de una venta. En este artículo aprenderás porque algunos cargos son denegados y qué medidas puedes tomar para bajar tu tasa de denegación.

Existen tres posibles razones por las cuales el cargo a una tarjeta puede ser denegado:

- Bancos y marcas de tarjetas
- Ventas riesgosas
- Peticiones inválidas

El motivo de denegación de un cargo es provisto a través del API y el Panel de Culqi como parte de la respuesta de un cargo a través del objeto Outcome. Estos parámetros incluyen el tipo de denegación obtenida y además información adicional acerca del motivo de rechazo.

### Denegaciones de Bancos y Marcas de Tarjetas

Cuando creas un Cargo en Culqi, el banco emisor y procesador de la marca de la tarjeta (Visa, MasterCard, etc.) determinan, a través de sistemas automatizados, si una transacción será autorizada o no, tomando en cuenta los hábitos de compra de tus clientes, el balance en sus cuentas bancarias y la información de sus tarjetas (como fecha de expiración y CVV), entre otros.

Si el banco emisor de la tarjeta o el procesador de la marca rechazan la transacción de tu cliente, Culqi compartirá a través del Panel y el API, el parámetro Decline Code con toda la información posible explicando el motivo de la denegación. En algunos casos, los bancos brindan explicaciones precisas sobre el rechazo. Por ejemplo, te avisaremos si la tarjeta no tiene fondos suficientes, si es una tarjeta robada o si la tarjeta está vencida, entre otros motivos.



Desafortunadamente, la mayoría de motivos son categorizados como genéricos por lo que no siempre es posible conocer porqué un cargo fue rechazado. Si la información de la tarjeta parece correcta, es mejor que tu cliente se contacte con su banco para solicitar mayor información. Cada IIN de tarjeta tiene privilegios y restricciones diferentes. Por privacidad y seguridad, los bancos pueden brindar el motivo específico del rechazo únicamente a su cliente, nunca al comercio.

Adicionalmente, a través del API puedes conocer el motivo de la denegación utilizando el objeto Outcome. En este objeto te brindamos el tipo de error, el código de respuesta y el código de la denegación. Adicionalmente te mostraremos un mensaje con más información para ti y una respuesta sugerida al cliente.

```json
...
, "outcome": {
    "object": "error",
    "type": "card_error",
    "charge_id": "chr_test_9IeSADjs6y0Y8S30",
    "code": "card_declined",
    "decline_code": "insufficient_funds",
    "merchant_message": "Fondos insuficientes. La tarjeta no tiene fondos suficientes para realizar la compra.",
    "user_message": "Su tarjeta no tiene fondos suficientes. Para realizar la compra, verifica tus fondos disponibles con el banco emisor o inténta nuevamente con otra tarjeta."
} ,
...
```

#### Códigos de Denegaciones de Bancos

Si un cargo es rechazado por el banco o la marca procesadora de la tarjeta, el banco puede proveer un código de denegación con un motivo mucho más detallado de la explicación. Debajo encontrarán una lista de posibles códigos de denegación que pueden ser devueltos.

#### Reduciendo denegaciones de Bancos y Marcas

Las denegaciones producidas por ingresar información errónea de la tarjeta (p.e., utilizar un número de tarjeta incorrecto o una fecha de expiración inválida) se pueden corregir guiando a tus clientes a modificar el dato ingresado, utilizar otra tarjeta e incluso utilizar otro medio de pago. Si utilizas nuestro Checkout , le brindaremos feedback instantáneo a tus clientes en el momento que están completando la información de la tarjeta, permitiéndoles corregir el error o utilizar un método de pago alternativo.

Las sospechas de los bancos por operaciones fraudulentas son más complejas de manejar. Recomendamos que siempre envíes toda la información que tengas de tu cliente. El código de seguridad (CVV), la dirección completa y cualquier información adicional que le sirva al banco para tomar una decisión correcta.


### Denegaciones de ventas muy riesgosas

El motor de análisis de fraude de Culqi bloqueará cualquier compra que identifique como de alto riesgo o que forme parte de de los usuarios o tarjetas reportadas por los bancos como fraudulentas. Por cada cargo que realices, te brindaremos un score de fraude que te permitirá conocer qué tan riesgosa es una transacción. Si el score es mayor o igual a 75% la compra será rechazada. Recomendamos que revises el score de cada cliente antes de realizar la entrega de tu producto o servicio.

Etiquetar Cliente


Usando el API, podrás obtener el score de fraude o fraud_score y la información de tu cliente a través del objeto cliente que te servirán para conocer el nivel de riesgo de una transacción.

#### Reduciendo denegaciones por ventas muy riesgosas

El motor de prevención de fraudes es una herramienta de inteligencia artificial automatizada que aprende del comportamiento fraudulento de tu negocio y rechazará los cargos que le parecen muy riesgosos.

Para que el motor se equivoque la menor cantidad de veces posible es indispensable etiquetar a tu clientes como Cliente de confianza¨ o ¨Cliente fraudulento¨ a través del Panel de Culqi.

Etiquetar Cliente 

Adicionalmente, es muy importante que nos envíes lo datos de tus clientes cada vez que realizas un cargo a través del objeto antifraud_details.

```json
{
  "source_id": "tkn_live_bk75nbyKfdGuiHvU",
  "email": "sdgfsdf@me.com",
  "currency_code": "PEN",
  "amount": 100000,
  "antifraud_details": {
    "address": "Avenida Lima 123",
    "address_city": "Lima",
    "country_code": "PE",
    "first_name": "Jimmy",
    "last_name": "Metha",
    "phone_number": 9912321123
  }
}
```

Enviar los mismos datos para todos tus clientes hará que la efectividad del motor baje ya que relacionará a más de un cliente con los mismos datos de usuario.

Si omites los detalles anti-fraude a la hora de crear un cargo o crear un cliente, Culqi no realizará una evaluación de riesgo de tu cliente y estos cargos podrían convertirse en un fraude o una disputa.

### Peticiones inválidos al API

Mientras realizas la integración con el servicio de Culqi, una buena práctica es identificar cualquier error potencial que provoque una petición inválida al API lo que te asegurará que estas fallas sean muy raras en el entorno de producción. En estos casos, no verás un intento de compra en el Panel, sino un error de autenticación con un mensaje que te guiará a corregir el problema.

```json
{
    "object": "error",
    "type": "{TIPO DE ERROR}",
    "code" : "{CODIGO DE ERROR CULQI}",
    "message": "{Descripción del error}",
    "user_message": "{Descripción del error para el usuario/cliente}"
}
```

### Controlar denegaciones automáticamente

Si deseas que tu integración responda a la falla de un cargo de manera automática, podrás acceder al objeto Outcome en dos maneras.

Maneja el objeto error del API que será devuelto cuando un cargo falla. Para denegaciones de bancos y por alto riesgo, el error devuelve el ID del cargo que podrás utilizar para consultar el cargo.
Haz uso de webhooks para recibir notificaciones de cualquier evento que quieras controlar. Cuando un cargo es denegado, el evento charge.failed es enviado a la URL de tu preferencia, conteniendo el objeto del cargo.