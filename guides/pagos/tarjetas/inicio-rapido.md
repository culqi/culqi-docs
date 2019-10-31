---
id: inicio-rapido
title: Inicio rápido
---

Culqi permite a los comercios poder aceptar pagos en línea con tarjetas de crédito, débito o prepagadas, de forma rápida y segura. Una vez capturada la información de la tarjeta, puedes crear un cargo inmediatamente o de forma recurrente.

> Antes de comenzar con esta guía rápida, recuerda que debes tener a disposición tus llaves de comercio disponibles en el Panel Culqi en el entorno de Integración o Producción.

La integración rápida se ha divido en dos simples pasos:

1. Crear un objeto token
2. Usar el id del objeto Token para crear un objeto Cargo

----

## Paso 1: Crear un objeto Token

El proceso de tokenización asegura que la información sensible de la tarjeta no toque tu servidor y tu integración pueda operar de forma compatible con la normativa PCI. Si en algún momento la información de la tarjeta llegara a pasar o ser almacenada en tu servidor, tendrías que hacerte responsable por cualquiera de las pautas y/o auditorías de PCI DSS.

Culqi proporciona las siguientes formas de tokenizar la información de pago de tu cliente de manera segura bajo HTTPS, para integrarte debes elegir cualquiera de ellas:

- Culqi Checkout (Recomendada, la más simple)
- Culqi.js (Checkout personalizado)
- Móviles (Android e iOS)

Para usar Culqi Checkout o Culqi JS debes tener instalado JQuery (librería de JavaScript)

### Culqi Checkout

Es la forma más rápida y simple de obtener los datos de tarjeta, además, brinda un formulario embebido con validaciones incluidas y un diseño amigable. Con esta implementación, cuando tus clientes ingresen la información de su tarjeta, será validada instantáneamente y luego tokenizada para su posterior uso en el servidor.

```
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
        amount: 3500
    });
    // Usa la funcion Culqi.open() en el evento que desees
    $('#buyButton').on('click', function(e) {
        // Abre el formulario con las opciones de Culqi.settings
        Culqi.open();
        e.preventDefault();
    });
</script>
```

¿Te interesa este método? Si es así, por favor consulta la información detallada de cómo integrar Culqi Checkout para comenzar.

### Culqi.js

Puedes crear un formulario personalizado de acuerdo al diseño o marca de tu sitio web. Culqi.js tokeniza la información de la tarjeta directamente con Culqi. Para saber más, consulta información detallada para construir un formulario de pago con Culqi.js.

### Móviles (iOS y Android)

Si vas a integrar Culqi para alguna plataforma móvil, puedes usar nuestra bibliotecas disponibles para iOS y Android. Culqi obtiene la información de la tarjeta de tu cliente y crea un token para que puedas usarlo en el servidor.

----

## Paso 2: Crear un objeto Cargo

Una vez creado un token, este debe ser enviado a tu servidor para realizar una petición al API de Culqi. Con este token tienes dos caminos:

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

MapString, Object metadata = new HashMapString, Object();
metadata.put("oder_id", "124");

MapString, Object charge = new HashMapString, Object();
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
    "address" => "Av. Lima 123",
    "address_city" => "Lima",
    "country_code" => "PE",
    "email" => "test_customer@culqi.com",
    "first_name" => "Test",
    "last_name" => "Culqi",
    "phone_number" => 999999999
  )
);
//Crear Tarjeta
$culqi->Cards->create(
  array(
    "customer_id" => "cus_test_gx0PEhDvHQDo0MKV",
    "token_id" => "tkn_live_VGfALn7UfSXZIWYf"
  )
);
//Crear Cargo
$culqi->Charges->create(
 array(
     "amount" => 2500,
     "currency_code" => "PEN",
     "email" => "test_charge@culqi.com",
     "source_id" => "crd_test_vzMuTHoueOMlgUPj"
   )
);
?>
```
<!--Java-->
```java
//Crear Cliente
Map customer = new HashMap();
customer.put("address","Av. Lima 123");
customer.put("address_city","Lima");
customer.put("country_code","PE");
customer.put("email","test_customer@culqi.com");
customer.put("first_name","Test");
customer.put("last_name","Culqi");
customer.put("phone_number", 999999999);
culqi.customer.create(customer);

//Crear Tarjeta
Map card = new HashMap();
card.put("customer_id","{cus_test_gx0PEhDvHQDo0MKV}");
card.put("token_id", "{tkn_live_VGfALn7UfSXZIWYf}");
culqi.card.create(card);

//Crear Cargo
Map charge = new HashMap();
Map metadata = new HashMap();
metadata.put("oder_id", "124");
charge.put("amount",2500);
charge.put("currency_code","PEN");
charge.put("email","test_charge@culqi.com");
charge.put("metadata", metadata);
charge.put("source_id", "{crd_live_VGfALn7UfSXZIWYf}");
culqi.charge.create(charge);
```
<!--Python-->
```python
# Crear Cliente
dir_refund = {
    'address': 'Av. Lima 123', 
    'address_city': 'lima', 'country_code': 'PE',
    'email': 'test_customer@culqi.com',
    'first_name': 'Test', 
    'last_name': 'Culqi',
    'phone_number': 999999999
}
culqipy.Customer.create(dir_refund)

# Crear Tarjeta
dir_card = {
    'customer_id': 'cus_test_gx0PEhDvHQDo0MKV', 
    'token_id': 'tkn_live_VGfALn7UfSXZIWYf'
}

culqipy.Card.create(dir_card)

# Crear Cargo
dir_charge = {
    'amount': 2500, 
    'currency_code': 'PEN',
    'email': 'test_charge@culqi.com',
    'installments':0, 
    "source_id": "crd_live_VGfALn7UfSXZIWYf"
}

culqipy.Charge.create(dir_charge)
```
<!--Ruby-->
```ruby
#Crear Cliente
Culqi::Customers.create(
    :address => "av lima 123",
    :address_city => "Lima",
    :country_code => "PE",
    :email => "test_customer@culqi.com",
    :first_name => "Test",
    :last_name => "Culqi",
    :phone_number => 999999999
)
#Crear Tarjeta
Culqi::Card.create(
      :customer_id => "cus_test_gx0PEhDvHQDo0MKV",
      :token_id => "tkn_live_VGfALn7UfSXZIWYf"
)
#Crear Cargo
Culqi::Charge.create(
      :amount => 2500,
      :email => "test_charge@culqi.com",
      :currency_code => "PEN",
      :source_id => "crd_live_VGfALn7UfSXZIWYf"
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

----

## Siguientes pasos

¡Felicitaciones! haz realizado tu primer cargo en Culqi, aquí hay unas cosas que pueden interesarte:

- Pruebas
- Pase a Producción