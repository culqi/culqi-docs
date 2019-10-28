---
id: capturar-tarjetas
title: Capturar Tarjetas
---

## Culqi Checkout

La manera más simple de empezar la integración de Culqi, está diseñado para que tus clientes tengan la mejor experiencia de compra rápida, segura y lista para realizar compras en tu sitio web.

Culqi Checkout es un formulario embebido que facilita la integración de Culqi en tu sitio web y móvil (adaptable, soporta tablets y smartphones). Además, te permite personalizar el título y subtítulo a mostrar.

Prueba haciendo click en el botón de abajo para desplegar el formulario del Checkout y disfrutar la experiencia Culqi.

Usa la siguiente tarjeta de prueba:


|                          |                            |
|--------------------------|----------------------------|
| **Tarjeta**                  | 4111 1111 1111 1111 (Visa) |
| **CVV**                      | 123                        |
| **Mes y año de vencimiento** | 09/2020                    | 

Puedes usar también el resto de tarjetas de prueba.

----

#### Integra Culqi Checkout en tu sitio web

Uno de las formas más rápidas y fáciles de tokenizar los datos de tarjeta es con Culqi Checkout, para integrarlo debes seguir los siguientes pasos:

1. Incluir Culqi Checkout para tokenizar la tarjeta
2. Muestra el Checkout de Culqi
3. Enviar el ID del objeto token hacia tu servidor

> Esta parte de la documentación, brinda unicamente los pasos para tokenizar la tarjeta de manera segura vía Culqi Checkout, en este punto no se efectúa el cobro.

Para realizar el cobro a la tarjeta tienes que crear el objeto Cargo. Puedes Revisar el flujo de un pago o una subscripción para tener una visión general.

Para usar Culqi Checkout o Culqi JS debes tener instalado JQuery (librería de JavaScript)

----

### Paso 1: Incluir Culqi Checkout para tokenizar la tarjeta

Para la tokenizar los datos de la tarjeta de forma segura, primero incluye el Culqi Checkout en tu página. Puedes incluirlo antes del cierre de la etiqueta `body`:

```html
<!-- Incluyendo Culqi Checkout -->
<script src="https://checkout.culqi.com/js/v3"></script>
</body>
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

Luego, configura los valores a mostrar en el formulario de Culqi Checkout.

```javascript
<script>
  Culqi.settings({
    title: 'Culqi Store',
    currency: 'PEN',
    description: 'Polo/remera Culqi lover',
    amount: 3500
  });
</script>
```

### Paso 2: Muestra el Checkout de Culqi

El segundo paso es mostrar el formulario de Culqi. En este paso el cliente ingresa la información de su tarjeta, y el formulario lo convierte en un token.

```javascript
$('#buyButton').on('click', function(e) {
    // Abre el formulario con la configuración en Culqi.settings
    Culqi.open();
    e.preventDefault();
});
```

### Paso 3: Enviar el ID del objeto token hacia tu servidor

Luego de haberse generado el token, este estará disponible en la función culqi(). Una vez que ha sido obtenido lo debes de enviar a tu servidor como mínimo el Id del token(token.id) para ser procesado y crear un cargo o una suscripción.

```javascript
function culqi() {
  if (Culqi.token) { // ¡Objeto Token creado exitosamente!
      var token = Culqi.token.id;
      alert('Se ha creado un token:' + token);
      //En esta linea de codigo debemos enviar el "Culqi.token.id"
      //hacia tu servidor con Ajax
  } else { // ¡Hubo algún problema!
      // Mostramos JSON de objeto error en consola
      console.log(Culqi.error);
      alert(Culqi.error.user_message);
  }
};
```

### Opciones de personalización para Culqi Checkout

Con la nueva funcionalidad de Culqi, podras personalizar tu formulario con mayor alcance. Recomendamos colocar esta funcionalidad entre Culqi.publicKey y Culqi.settings.

A continuación proporcionaremos un ejemplo e información de los parametros y valores que puedes usar. Cualquier parámetro o valor no provisto será omitido.

```javascript
Culqi.options({
    lang: 'auto',
    modal: false,
    installments: true,
    customButton: 'Donar',
    style: {
      logo: 'https://culqi.com/LogoCulqi.png',
      maincolor: '#0ec1c1',
      buttontext: '#ffffff',
      maintext: '#4A4A4A',
      desctext: '#4A4A4A'
    }
});
```

| Parámetro    | Tipo    | Descripción                      |
|--------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| lang         | string  | Idioma del checkout Ejemplo: "auto", "en", "es"                                                                                                                          |
| modal        | boolean | Indica si el checkout es modal incrustrado o no <br/>  Ejemplo: "true", "false" <br/> Para no tener errores, por favor revisar este ejemplo                                           |
| installments | boolean | Indica si se activa o no la opción de pago en cuotas <br/>  Ejemplo: "true", "false" <br/>  Recuerda que esto es solo una de las partes para tener el flujo completo de pago en cuotas |
| customButton | string  | Permite colocar el texto que desees <br/>  Ejemplo: "Donar", "Paga aqui 10.00 PEN"                                                                                              |
| style        | object  | Permite personalizar los colores y logo de Culqi Checkout <br/>  Por ejemplo podras cambiar el color del botón del Culqi Checkout                                               |

A continuación proporcionaremos los parámetros y posibles valores para el objeto **style**

| Parámetro  | Tipo   | Descripción                                                                               |
|------------|--------|-------------------------------------------------------------------------------------------|
| logo       | string | Indica la URL del logo que se desea configurar <br/> Ejemplo: "https://culqi.com/LogoCulqi.png" |
| maincolor  | string | Indica el color del botón <br/> Ejemplo: "#9BB613"                                              |
| buttontext | string | Indica el color del texto del botón <br/> Ejemplo: "#080A01"                                    |
| maintext   | string | Indica el color del título de Culqi Checkout <br/> Ejemplo: "#2325B5"                           |
| desctext   | string | Indica el color de la descripción de Culqi Checkout <br/> Ejemplo: "#FE0000"                    |


## Culqi JS

Si quieres brindarles a tus clientes una experiencia personalizada de compra, entonces debes de usar Culqi JS.

----

Culqi JS facilita la integración de Culqi en tu sitio web y móvil. Además, te permite personalizar todo el formulario de Culqi.

Prueba haciendo click en el botón de abajo para crear el objeto Token y disfrutar la experiencia Culqi.

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

Este tutorial brinda los pasos solamente para tokenizar la tarjeta de manera segura vía Culqi JS, en este punto no efectúa cobros. Para luego realizar cobros a la tarjeta tienes que crear cargos. Revisar el flujo de un pago o una subscripción para tener una visión general.

Para usar Culqi Checkout o Culqi JS debes tener instalado JQuery (librería de JavaScript)

### Paso 1: Desarrolla tu checkout personalizado con Culqi JS

El primer paso es crear el formulario de captura de la tarjeta. En este paso el cliente ingresa la información de su tarjeta, para que Culqi JS pueda crear el objeto Token usando la función **Culqi.createToken()**

```html
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
<button id="btn_pagar">Pagar</button>
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

```javascript
<script>
  $('#btn_pagar').on('click', function(e) {
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
        //En esta linea de codigo debemos enviar el "Culqi.token.id"
        //hacia tu servidor con Ajax
    } else { // ¡Hubo algún problema!
        // Mostramos JSON de objeto error en consola
        console.log(Culqi.error);
        alert(Culqi.error.user_message);
    }
  };
```

## Culqi Android

Es la manera más simple de empezar la integración de Culqi, está diseñado para que tus clientes tengan la mejor experiencia de compra rápida y segura.

----

Uno de las formas más rápidas y fáciles de tokenizar los datos de tarjeta desde una aplicación móvil es con Culqi Android, para integrarlo debes seguir los siguientes pasos:

1. Instalar
2. Crear un Token
3. Enviar el Token a tu servidor

Este tutorial brinda los pasos solo para tokenizar la tarjeta de manera segura vía Culqi Android, en este punto no efectúa cobros. Para luego realizar cobros a la tarjeta tienes que crear cargos. Revisar el flujo de un pago o una subscripción para tener una visión general.

----

### Paso 1: Instalar

Para tokenizar los datos de la tarjeta de forma segura desde un dispositivo Android, primero debes instalarlo en tu proyecto. Puedes aprender a hacerlo aquí.

----

### Paso 2: Crear un Token

Si crea su propio formulario de pago, deberás recopilar los números de tarjeta, las fecha de vencimiento, el cvv y el correo electrónico del cliente. Una vez que haya recopilado la información de un cliente, debes de crear una petición para crear el token de la tarjeta.

Puede crear tokens utilizando el método de instancia Culqi createToken pasando la información de la tarjeta

<!--DOCUSAURUS_CODE_TABS-->
<!--Java-->
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
<!--END_DOCUSAURUS_CODE_TABS-->

----

### Paso 3: Enviar el Token a tu servidor

El uso de token requiere una petición al API de Culqi desde tu servidor utilizando su llave secreta. (Por razones de seguridad, nunca debes de incrustar la llave secreta en la aplicación.) El método 'createToken' devuelve en 'onSuccess' una respuesta en JSON y en onError el error de la petición HTTP. Una vez obtenido el token lo puedes usar para crear una suscripción o un cargo.


## Culqi iOS

Es la manera más simple de empezar la integración de Culqi en dispositivos iOS, está diseñado para que tus clientes tengan la mejor experiencia de compra rápida y segura.

Uno de las formas más rápidas y fáciles de tokenizar los datos de tarjeta desde una aplicación móvil es con Culqi iOS, para integrarlo debes seguir los siguientes pasos:

1. Instalarlo
2. Crear un Token
3. Enviar el Token a tu servidor

Este tutorial brinda los pasos solamente para tokenizar la tarjeta de manera segura vía Culqi iOS, en este punto no efectúa cobros. Para luego realizar cobros a la tarjeta tienes que crear cargos. Revisar el flujo de un pago o una subscripción para tener una visión general.

----

### Paso 1: Instalarlo

Para tokenizar los datos de la tarjeta seguramente desde un dispositivo iOS, primero debes instalarlo en tu proyecto. Puedes aprender a hacerlo aquí.

----

### Paso 2: Crear un Token

Si crea su propio formulario de pago, deberás recopilar los números de tarjeta, las fecha de vencimiento, el cvv y el correo electrónico del cliente. Una vez que haya recopilado la información de un cliente, debes de crear una petición para crear el token de la tarjeta.

Puede crear tokens utilizando el método utilizando el método de instancia Culqi createToken Pasando el número de la tarjeta, cvv, la fecha de vencimiento y un correo electrónico.

<!--DOCUSAURUS_CODE_TABS-->
<!--Objective C-->
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
<!--END_DOCUSAURUS_CODE_TABS-->

----


### Paso 3: Enviar el Token a tu servidor

El uso de token requiere una petición al API de Culqi desde tu servidor utilizando su llave secreta. (Por razones de seguridad, nunca debes de incrustar la llave secreta en la aplicación.) El método 'createToken' devuelve en 'onSuccess' una respuesta en JSON y en onError el error de la petición HTTP. Una vez obtenido el token lo puedes usar para crear una suscripción o un cargo.