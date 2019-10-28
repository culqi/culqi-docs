---
id: capturar-pagos
title: Capturar Pagos
---

## Culqi Checkout

La manera más simple de empezar la integración de Culqi, está diseñado para que tus clientes tengan la mejor experiencia de compra rápida, segura y lista para realizar compras en tu sitio web.

Culqi Checkout es un formulario embebido que facilita la integración de Culqi en tu sitio web y móvil (adaptable, soporta tablets y smartphones) con múltiples medios de pago (tarjetas y PagoEfectivo). Además, te permite personalizar el título y subtítulo a mostrar.

Prueba haciendo click en el botón de abajo para desplegar el formulario del Checkout y disfrutar la experiencia Culqi.

Usa la siguiente tarjeta de prueba:

|                          |                            |
|--------------------------|----------------------------|
| **Tarjeta**                  | 4111 1111 1111 1111 (Visa) |
| **CVV**                      | 123                        |
| **Mes y año de vencimiento** | 09/2020                    | 


---- 

#### Integra Culqi Checkout en tu sitio web

Uno de las formas más rápidas y fáciles de tokenizar los datos de tarjeta es con Culqi Checkout, para integrarlo debes seguir los siguientes pasos:

1. Incluir Culqi Checkout Multipago
2. Crea un token de uso único
3. Enviar el token hacia tu servidor u obtener codigo de pago

> Esta parte de la documentación, brinda unicamente los pasos para tokenizar la tarjeta de manera segura vía Culqi Checkout, en este punto no se efectúa el cobro.

Para realizar el cobro a la tarjeta tienes que crear el objeto Cargo. Puedes Revisar el flujo de un pago o una subscripción para tener una visión general.

----

### Paso 1: Incluir Culqi Checkout para tokenizar la tarjeta

Para la tokenizar los datos de la tarjeta de forma segura, primero incluye el Culqi Checkout en tu página. Puedes incluirlo antes del cierre de la etiqueta "body":

```javascript
<!-- Incluyendo Culqi Checkout -->
<script src="https://checkout.culqi.com/js/v3"></script>
```

> El .js del checkout siempre debe ser incluido desde https://checkout.culqi.com/js/v3 . Usar una copia local no está soportado y puede resultar en errores visibles para el usuario.

En otra etiqueta, debajo del llamado al Culqi Checkout, configura tu llave pública:

```javascript
<script>
  Culqi.publicKey = 'Aquí inserta tu llave pública';
</script>
```

Tu llave pública nos permite identificar las comunicaciones entre tu cliente (sitio web) y Culqi. Recuerda reemplazarlo con tu propia llave pública del ambiente de integración.

> Para obtener tu llave pública ingresa a la sección Desarrollo > API Keys ubicada en el Panel de Culqi.

Luego, configura los valores a mostrar en el formulario de Culqi Checkout. Es importante también añadir el order id obtenido en el primer paso del flujo multipago, solo de esta manera funcionará el Culqi Checkout Multipago.

El id de Order lo tendrás que añadir dinamicamente con cada creación de orden previa e inscrustar en la configuración del checkout.

```javascript 
<script>
  Culqi.settings({
    title: 'Culqi Store',
    currency: 'PEN',
    description: 'Polo/remera Culqi lover',
    amount: 3500,
    order: 'ord_live_0CjjdWhFpEAZlxlz'
  });
</script>
```

### Paso 2: Muestra el Checkout de Culqi

El segundo paso es mostrar el formulario de Culqi. En este paso el cliente ingresa la información de su tarjeta, y el formulario lo convierte en un token.

```html
$('#buyButton').on('click', function(e) {
    // Abre el formulario con la configuración en Culqi.settings
    Culqi.open();
    e.preventDefault();
});
```

### Paso 3: Enviar el ID del objeto token hacia tu servidor o recibir objeto orden

Luego de haberse generado el token, este estará disponible en la función culqi(). Una vez que ha sido obtenido lo debes de enviar a tu servidor como mínimo el Id del token(token.id) para ser procesado y crear un cargo o una suscripción.

Por otro lado, si tu cliente optó por elegir PagoEfectivo. Recibirás el objeto orden en la función, como accion sugerida, deberás hacer que tu pedido pase a un estado pendiente y, además, guardes el id de la orden dentro de tu pedido. Esto es para que luego pueda ser trazable cuando te llegue el evento de confirmación de pago de la orden via webhook.

Se recomienda usar AJAX para enviar la información hacia su servidor desde el cliente.

```javascript
function culqi() {
  if (Culqi.token) { // ¡Objeto Token creado exitosamente!
      var token = Culqi.token.id;
      alert('Se ha creado un token:' + token);
      // Aqui enviar token Id a servidor para crear cargo...
  }
  else if (Culqi.order) {
      console.log(Culqi.order)

      alert('Se ha elegido el metodo PagoEfectivo');

       /* Aqui enviar al servidor el order Id y asociarlo al detalle de tu venta.
          Además, tu venta en tu comercio debe quedar estado pendiente.
        */
  }
  else { // ¡Hubo algún problema!
      // Mostramos JSON de objeto error en consola
      console.log(Culqi.error);
      alert(Culqi.error.user_message);
  }
};
```