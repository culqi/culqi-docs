---
id: inicio-rapido
title: Inicio rápido
---

Culqi también permite a los comercios aceptar pagos con diferentes medios: con tarjetas de crédito/débito y con efectivo de forma práctica. Dando asì a tu cliente la opción de pagar mediante varias maneras.

Una vez es capturada la información de la tarjeta, se crea el cargo inmediatamente (cobro a tarjeta). Si la creación del cargo falla, se sugiere el método de PagoEfectivo. De esta forma, la venta es recuperada y con más probabilidad de ser culminada.

> Antes de comenzar con esta guía rápida, recuerda que debes tener a disposición tus llaves de comercio disponibles en el Panel Culqi en el entorno de Integración o Producción.

La integración rápida de multipagos se puede resumir en cuatro pasos:

1. Crear una orden
2. Integración de Culqi Checkout
3. Crear un cargo (usando token de tarjeta)
4. Recibir eventos de la orden

## Paso 1: Crear una orden

El primer paso para el uso de pagos mediante varios métodos, es crear una orden. Una orden contiene todos los detalles básicos de la venta y es usado solo por el medio de PagoEfectivo. Servirá para que después se pueda adjuntar el id dentro de la configuración del Checkout Multipago (paso 2). Esta orden debe ser creada usando el parámetro confirm:false.

Para ver información detallada de este paso: creación de orden.


<!--DOCUSAURUS_CODE_TABS-->
<!--PHP-->
```php 
<?php
    $order = $culqi->Orders->create(
       array(
         "amount" => 1000,
         "currency_code" => "PEN",
         "description" => 'Venta de prueba',
         "order_number" => 'pedido-9999',
         "client_details" => array(
            "first_name"=> "richard",
            "last_name" => "Hendricks",
            "email" => "richard.hendricks@piedpiper.com",
            "phone_number" => "+51945145222"
          ),
          "confirm" => false,
          "expiration_date" => time() + 24*60*60   // 1 dia
       )
    );
?>
```
<!--Curl-->
```bash
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'authorization: Bearer pk_test_vzMuTHoueOMlgUPj' -d '{
  "amount": 1000,
      "currency_code": "PEN",
      "description": "Venta de prueba",
      "order_number": "pedido-9999",
      "client_details" : {
        "first_name": "Richard",
        "last_name": "Hendricks",
        "email": "richard@piedpiper.com",
        "phone_number": "+51945145280"
      },
  "expiration_date": 2323423432,
  "confirm": false
}' 'https://api.culqi.com/v2/orders'
```
<!--END_DOCUSAURUS_CODE_TABS-->

## Paso 2: Integración de Culqi Checkout

El proceso de tokenización asegura que la información sensible de la tarjeta no toque tu servidor y tu integración pueda operar de forma compatible con la normativa PCI. Si en algún momento la información de la tarjeta llegara a pasar o ser almacenada en tu servidor, tendrías que hacerte responsable por cualquiera de las pautas y/o auditorías de PCI DSS.

Culqi proporciona el Checkout Culqi para tokenizar la información de pago de tu cliente de manera segura bajo HTTPS cuando se trate de pago con tarjetas. En cuanto a los medios de pago adicionales, estos también se muestran dentro del mismo checkout.

### Culqi Checkout (Multipago)

Es la forma más rápida y simple de obtener los datos de tarjeta, además, brinda un formulario embebido con validaciones incluidas y un diseño amigable. Con esta implementación, cuando tus clientes ingresen la información de su tarjeta, será validada instantáneamente y luego tokenizada para su posterior uso en el servidor. Además, al ser la modalidad multipago, podrá redirigir al usuario a PagoEfectivo ante cualquier inconveniente con el pago con tarjetas.

```javascript
<!-- Incluye Culqi Checkout en tu sitio web-->
<script src="https://checkout.culqi.com/js/v3"></script>

<script>
    // Configura tu llave pública
    Culqi.publicKey = 'Aquí inserta tu llave pública';
    // Configura tu Culqi Checkout
    Culqi.settings({
        title: 'Culqi Store',
        currency: 'PEN',
        description: 'Polo Culqi lover',
        amount: 3500,
        order: "ord_live_0CjjdWhFpEAZlxlz"
    });
    // Usa la funcion Culqi.open() en el evento que desees
    $('#buyButton').on('click', function(e) {
        // Abre el formulario con las opciones de Culqi.settings
        Culqi.open();
        e.preventDefault();
    });
</script>
```

Para ver información detallada de este paso: cómo integrar Culqi Checkout (Multipagos)

----

## Paso 3: Crear un cargo (mediante token de tarjeta)

Una vez configurado el Culqi Checkout Multipago, si es que el usuario usa el método de pago con tarjeta, generará un token de tarjeta. Este token tiene que ser usado luego para Crear un Cargo. Por lo que tendrá que ser enviado a tu servidor al momento que recibas el evento del checkout.

- Crear un cargo único
- Crear cargos posteriores (One Click Payments)

### Crear un cargo único

Puedes crear una petición de cargo único a la tarjeta de tu cliente ni bien se obtiene el token, y con ello se termina el flujo básico de la implementación de pagos con tarjeta. La petición de cargo contiene el token, código de la moneda, monto del cargo, cuotas, captura (autorización y captura) e información adicional que requieras adjuntar (ver metadata). Usando este camino, el cliente necesita reingresar la información de pago en cada compra. Para ver más información detallada: creación de cargos.


<!--DOCUSAURUS_CODE_TABS-->
<!--PHP-->
```php 
<?php
$charge = $culqi->Charges->create(
 array(
     "amount" => 1500,
     "currency_code" => "PEN",
     "email" => "test_charge@culqi.com",
     "source_id" => "id del objeto Token o id del objeto Card"
   )
);
?>
```
<!--Java-->
```java
import java.util.Scanner;
MapString, Object charge = new HashMapString, Object();
MapString, Object metadata = new HashMapString, Object();
metadata.put("oder_id", "124");
charge.put("amount", 1500);
charge.put("currency_code","PEN");
charge.put("email","test_charge@culqi.com");
charge.put("source_id", "id del objeto Token o id del objeto Card");
charge.put("metadata", metadata);
culqi.charge.create(charge);
```
<!--Python-->
```python
dir_charge = {
    "amount": 1500, 
    "currency_code": "PEN",
    "email": "test_charge@culqi.com",
    "source_id": "id del objeto Token o id del objeto Card"
}

culqipy.Charge.create(dir_charge)
```
<!--Ruby-->
```ruby
Culqi::Charge.create(
    :amount => 1500,
    :email => "test_charge@culqi.com",
    :currency_code => "PEN",
    :source_id => "id del objeto Token o id del objeto Card"
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
    {"amount", 1500},
    {"currency_code", "PEN"},
    {"email", "test_charge@culqi.com"},
    {"metadata", metadata},
    {"source_id", "id del objeto Token o id del objeto Card"}
};
new Charge(security).Create(map);
```
<!--END_DOCUSAURUS_CODE_TABS-->

### Crear cargos posteriores

Culqi te ofrece la posibilidad de realizar cobros a las tarjetas de tus clientes sin que reingresen la información de la tarjeta. Este proceso consiste en almacenar la información del comprador en un objeto Cliente y luego crear un objeto Tarjeta.

Los cargos futuros se realizan con el Id de la Tarjeta creada. Este camino permite implementar lo que se conoce como "One Click Payments" facilitando la compra rápida en tu sitio.


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

## Paso 4: Recibir eventos de la orden

Recordando que el pago con tarjetas es un medio en tiempo real y la respuesta se recibe en el instante para los pagos con tarjetas no es necesario los webhooks. Por otro lado, PagoEfectivo es un medio de pago asíncrono, del cual para poder saber la respuesta final (exitosa o expirada) es obligatorio implementar un webhook para recibir los eventos de cambio de estado de la orden.

Para ver más información detallada de este paso: eventos de orden.

----

## Siguientes pasos

¡Felicitaciones! haz realizado tu integración multipagos de Culqi, aquí hay unas cosas que pueden interesarte:

- Flujo de multipago
- Pase a Producción