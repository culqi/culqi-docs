---
id: crear-orden
title: Crear Orden
---

## Crear orden

La creación de una orden comprende el primer paso del flujo de multipagos en Culqi, en este paso se crea una orden. Debes crear desde tu comercio una orden con los detalles de la venta (mediante Culqi API o bibliotecas).

Por otro lado, es necesario indicar que a una orden se le asigna un tiempo máximo de pago (expiración) que pueden ser horas o días. Esto es el tiempo máximo que se le da al usuario para que pueda realizar el pago de la orden.

> Este tutorial asume que ya has implementado una solución para capturar la información de las tarjetas de tus clientes, usando Culqi Checkout. Si aún no lo has hecho, revisa el inicio rápido de multipagos, donde encontrarás paso a paso todo lo que necesitas para implementar el flujo.

----

### Crear una orden para el checkout

Una orden contiene todos los detalles básicos de la venta y es usado solo para PagoEfectivo. Servirá para que después se pueda adjuntar el id dentro de la configuración del Checkout Multipago (paso 2).

> Esta orden debe ser creada usando el parámetro confirm:false. Debido a que el usuario confirmará la orden dentro del Culqi Checkout.


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
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json'
--header 'authorization: Bearer pk_test_vzMuTHoueOMlgUP' -d '{
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

Luego, una vez confirmada la orden, automaticamente Culqi enviará un correo con las instrucciones de pago al cliente. Debido a que tenemos que esperar el pago por el lado del cliente, en los siguiente pasos veremos como manejamos los cambios de estado mediante eventos. Por ejemplo: Cuando una orden es pagada o expirada.

----

### Añadir información adicional a la orden

Con Culqi puedes añadir metadata a la mayoría de las peticiones de creación, y una de ellas es crear orden. La información en este campo es para tu uso interno. Puedes añadir, por ejemplo: Información de edad, documento de identificación del cliente, etc.

Toda la metadata se encuentra visible en el Panel Culqi, al visualizar el detalle de una orden y está disponible para ser exportada. Haciendo un pequeño ejemplo, esto permite que tu equipo de contabilidad o finanzas puede fácilmente conciliar las órdenes en Culqi con los pedidos en tu sistema:

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
          "expiration_date" => time() + 24*60*60,   // 1 dia
          "metadata" => array ("dni" => "71701976" )
       )
    );
?>
```
<!--Curl-->
```bash
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json'
--header 'authorization: Bearer pk_test_vzMuTHoueOMlgUP' -d '{
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
  "confirm": false,
  "metadata": {"ORDER_ID": "123"}
}' 'https://api.culqi.com/v2/orders'
```
<!--END_DOCUSAURUS_CODE_TABS-->


## Siguientes pasos:

¡Buen trabajo! Si has llegado hasta este punto ya sabes un poco más de como crear órdenes. 
Pueden interesarte los siguientes artículos:

- Checkout Multipago
