---
id: capturar-tarjetas
title: Capturar Tarjetas
---

## Culqi Checkout para suscripciones

En este caso usaremos Culqi Checkout para "Guardar" la tarjeta del cliente, al ser una suscripción el cliente debe ser consciente que no esta pagando si no que esta "Guardando" su tarjeta.

Crearemos el objeto Token con Culqi Checkout para que el ID del objeto Token pueda ser enviado a tu servidor.

Usa la siguiente tarjeta de prueba


|                          |                            |
|--------------------------|----------------------------|
| **Tarjeta**                  | 4111 1111 1111 1111 (Visa) |
| **CVV**                      | 123                        |
| **Mes y año de vencimiento** | 09/2020                    | 


Integra Culqi Checkout en tu sitio web

Para capturar la tarjeta con el botón "Guardar" en el Checkout debes seguir los siguientes pasos:

1. ncluir Culqi Checkout (Guardar) para tokenizar
2. Crear el objeto Token
3. Enviar el ID del objeto token hacia tu servidor

Este tutorial brinda los pasos solamente para tokenizar la tarjeta de manera segura vía Culqi Checkout y colocando la palabra "GUARDAR" en el checkout, en este punto no efectúa cobros. Para luego realizar la creación del objeto Cliente y objeto Tarjeta en tu servidor.

### Paso 1: Incluir Culqi Checkout (Guardar) para tokenizar

Para la tokenizar los datos de la tarjeta de forma segura, primero incluye el Checkout.js en tu página. Puedes incluirlo antes de cerrar la etiqueta "body":

```
<!-- Incluyendo .js de Culqi Checkout-->
<script src="https://checkout.culqi.com/js/v3"></script>
</body>
```

> El .js del checkout siempre debe ser incluido desde https://checkout.culqi.com/js/v3 . Usar una copia local no está soportado y puede resultar en errores visibles para el usuario.

En otra etiqueta, debajo del llamado al .js, configura tu llave pública:

```javascript
<!-- Configurando el checkout-->
<script>
    Culqi.publicKey = '<Aquí inserta tu llave pública>';
</script>
```

Tu llave pública nos permite identificar las comunicaciones entre tu cliente (sitio web) y Culqi. Recuerda reemplazarlo con tu propia llave pública del ambiente de integración.

> Para obtener tu llave pública ingresa a la sección Desarrollo > API Keys ubicada en el Panel de Culqi.

Luego, configura los valores a mostrar en el formulario Culqi Checkout. El parametro "amount" no debe tener un valor para que el botón del Checkout de Culqi tenga la palabra "GUARDAR"

```
<!-- Configurando el checkout-->
<script>
    Culqi.settings({
        title: 'Culqi Store',
        currency: 'PEN',
        description: 'Suscripción Culqi lover',
        amount: ""
    });
</script>
```

### Paso 2:  Crear el objeto Token

El segundo paso es mostrar el formulario de Culqi, este captura la información de la tarjeta del cliente y lo convierte en un token.

```
$('#buyButton').on('click', function(e) {
            // Abre el formulario con las opciones de Culqi.settings
            Culqi.open();
            e.preventDefault();
});
```

### Paso 3: Enviar el ID del objeto token hacia tu servidor

Luego de haberse generado el token, este estará disponible en la función culqi(). Una vez que ha sido obtenido lo debes de enviar a tu servidor para ser procesado y crear una suscripción.

```javascript
function culqi() {

    if(Culqi.token) { // ¡Token creado exitosamente!
        // Get the token ID:
        var token = Culqi.token.id;
        alert('Se ha creado un token:'.token);

    }else{ // ¡Hubo algún problema!
        // Mostramos JSON de objeto error en consola
        console.log(Culqi.error);
        alert(Culqi.error.mensaje);
    }
};
```


## Culqi JS para subscripciones

Culqi JS facilita la integración de Culqi en tu sitio web y móvil. Además, te permite personalizar todo el formulario de Culqi.

Prueba haciendo click en el botón de abajo para desplegar el formulario del Checkout y disfrutar la experiencia Culqi.

Usa la siguiente tarjeta de prueba:

|                          |                            |
|--------------------------|----------------------------|
| **Tarjeta**                  | 4111 1111 1111 1111 (Visa) |
| **CVV**                      | 123                        |
| **Mes y año de vencimiento** | 09/2020                    | 

#### Integra Culqi JS en tu sitio web

Uno de las formas más rápidas y fáciles de tokenizar los datos de tarjeta es con Culqi Checkout, para integrarlo debes seguir los siguientes pasos:

1. Desarrolla tu checkout personalizado con Culqi JS
2. Integra Culqi JS y crea un objeto Token
3. Enviar el token hacia tu servidor

Este tutorial brinda los pasos solamente para tokenizar la tarjeta de manera segura vía Culqi JS, en este punto no efectúa cobros. Para luego realizar cobros a la tarjeta tienes que crear cargos. Revisa el flujo de una subscripción para tener una visión general.


### Paso 1: Desarrolla tu checkout personalizado con Culqi JS

El primer paso es crear el formulario de captura de la tarjeta. En este paso el cliente ingresa la información de su tarjeta, para que Culqi JS pueda crear el objeto Token usando la función Culqi.createToken()

```
<form>
  <div>
    <label>
      <span>Correo Electrónico</span>
      <input type="text" size="50" data-culqi="card[email]" id="card[email]">
    </label>
  </div>
  <div>
    <label>
      <span>Número de tarjeta</span>
      <input type="text" size="20" data-culqi="card[number]" id="card[number]">
    </label>
  </div>
  <div>
    <label>
      <span>CVV</span>
      <input type="text" size="4" data-culqi="card[cvv]" id="card[cvv]">
    </label>
  </div>
  <div>
    <label>
      <span>Fecha expiración (MM/YYYY)</span>
      <input size="2" data-culqi="card[exp_month]" id="card[exp_month]">
      <span>/</span>
      <input size="4" data-culqi="card[exp_year]" id="card[exp_year]">
    </label>
  </div>
</form>
```

### Paso 2: Integra Culqi JS y crea un objeto Token

Para tokenizar los datos de la tarjeta de forma segura, primero incluye el JS en tu página. Puedes incluirlo antes del cierre de la etiqueta "body" o dentro de la etiqueta "head":

```
<!-- Incluyendo .js de Culqi JS -->
<script src="https://checkout.culqi.com/v2"></script>
```

> El JS de Culqi siempre debe ser incluido desde https://checkout.culqi.com/v2 . Usar una copia local no está soportado y puede resultar en errores visibles para el usuario.

En otra etiqueta, debajo del llamado al JS, configura tu llave pública:

```javascript
<script>
    Culqi.publicKey = 'Aquí inserta tu llave pública';
    Culqi.init();
</script>
```

Tu llave pública nos permite identificar las comunicaciones entre tu cliente (sitio web) y Culqi. Recuerda reemplazarlo con tu propia llave pública del ambiente de integración.

> Para obtener tu llave pública ingresa a la sección Desarrollo > API Keys ubicada en el Panel de Culqi.

Luego usa la función "Culqi.createToken" en el evento que desees.

```
<script>
  $('#buyButton').on('click', function(e) {
      // Crea el objeto Token con Culqi JS
      Culqi.createToken();
      e.preventDefault();
  });
</script>
```

### Paso 3: Enviar el token hacia tu servidor

Luego de haberse generado el token, este estará disponible en la función culqi(). Una vez que ha sido obtenido lo debes de enviar a tu servidor como mínimo el Id del token(token.id) para ser procesado y crear un cargo o una suscripción.

```javascript
function culqi() {
    if (Culqi.token) { // ¡Objeto Token creado exitosamente!
        var token = Culqi.token.id;
        alert('Se ha creado un token:' + token);
    } else { // ¡Hubo algún problema!
        // Mostramos JSON de objeto error en consola
        console.log(Culqi.error);
        alert(Culqi.error.user_message);
    }
  };
```

## Culqi Android

Es la manera más simple de empezar la integración de Culqi, está diseñado para que tus clientes tengan la mejor experiencia de compra rápida y segura.

Uno de las formas más rápidas y fáciles de tokenizar los datos de tarjeta desde una aplicación móvil es con Culqi Android, para integrarlo debes seguir los siguientes pasos:

1. Instalar
2. Crear un Token
3. Enviar el Token a tu servidor

Este tutorial brinda los pasos solo para tokenizar la tarjeta de manera segura vía Culqi Android, en este punto no efectúa cobros. Para luego realizar cobros a la tarjeta tienes que crear cargos. Revisar el flujo de un pago o una subscripción para tener una visión general.

### Paso 1: Instalar

Para tokenizar los datos de la tarjeta de forma segura desde un dispositivo Android, primero debes instalarlo en tu proyecto. Puedes aprender a hacerlo aquí.

### Paso 2: Crear un Token

Si crea su propio formulario de pago, deberás recopilar los números de tarjeta, las fecha de vencimiento, el cvv y el correo electrónico del cliente. Una vez que haya recopilado la información de un cliente, debes de crear una petición para crear el token de la tarjeta.

Puede crear tokens utilizando el método de instancia Culqi createToken pasando la información de la tarjeta

```java
Card card = new Card(“411111111111111”, “123”, 9, 2020, “prueba_android@culqi.com”);

Token token = new Token("{LLAVE PUBLICA}");

token.createToken(getApplicationContext(), card, new TokenCallback() {
      @Override
      public void onSuccess(JSONObject token) {
            // token
            token.get("id").toString()
      }

      @Override
      public void onError(Exception error) {
      }
});
```

### Paso 3: Enviar el Token a tu servidor

El uso de token requiere una petición al API de Culqi desde tu servidor utilizando su llave secreta. (Por razones de seguridad, nunca debes de incrustar la llave secreta en la aplicación.) El método 'createToken' devuelve en 'onSuccess' una respuesta en JSON y en onError el error de la petición HTTP. Una vez obtenido el token lo puedes usar para crear una suscripción o un cargo.



## Culqi iOS

Es la manera más simple de empezar la integración de Culqi en dispositivos iOS, está diseñado para que tus clientes tengan la mejor experiencia de compra rápida y segura.

Uno de las formas más rápidas y fáciles de tokenizar los datos de tarjeta desde una aplicación móvil es con Culqi iOS, para integrarlo debes seguir los siguientes pasos:

- Instalarlo
- Crear un Token
- Enviar el Token a tu servidor

Este tutorial brinda los pasos solamente para tokenizar la tarjeta de manera segura vía Culqi iOS, en este punto no efectúa cobros. Para luego realizar cobros a la tarjeta tienes que crear cargos. Revisar el flujo de un pago o una subscripción para tener una visión general.

### Paso 1: Instalarlo

Para tokenizar los datos de la tarjeta seguramente desde un dispositivo iOS, primero debes instalarlo en tu proyecto. Puedes aprender a hacerlo aquí.

### Paso 2: Crear un Token

Si crea su propio formulario de pago, deberás recopilar los números de tarjeta, las fecha de vencimiento, el cvv y el correo electrónico del cliente. Una vez que haya recopilado la información de un cliente, debes de crear una petición para crear el token de la tarjeta.

Puede crear tokens utilizando el método utilizando el método de instancia Culqi createToken Pasando el número de la tarjeta, cvv, la fecha de vencimiento y un correo electrónico.

```objectivec
CLQCard *card = [CLQCard newWithNumber:[numberFormatter numberFromString:self.txtFieldCardNumber.text]
CVC:[numberFormatter numberFromString:self.txtFieldCVC.text]
expMonth:[numberFormatter numberFromString:self.txtFieldExpMonth.text]
expYear:[numberFormatter numberFromString:self.txtFieldExpYear.text]
email:self.txtFieldEmail.text];

[[Culqi sharedInstance] createTokenForCard:card success:^(CLQToken * _Nonnull token) {
    NSLog(@"Did create token with identifier: %@", token.identifier);
} failure:^(NSError * _Nonnull error) {
    NSLog(@"Error Creating token: %@", error.localizedDescription);
}];
```

### Paso 3: Enviar el Token a tu servidor

El uso de token requiere una petición al API de Culqi desde tu servidor utilizando su llave secreta. (Por razones de seguridad, nunca debes de incrustar la llave secreta en la aplicación.) El método 'createToken' devuelve en 'onSuccess' una respuesta en JSON y en onError el error de la petición HTTP. Una vez obtenido el token lo puedes usar para crear una suscripción o un cargo.



