---
id: tokenizacion
title: Tokenización
---

## Paso 1: Crear un objeto Token

El proceso de tokenización asegura que la información sensible de la tarjeta no toque tu servidor y tu integración pueda operar de forma compatible con la normativa PCI. Si en algún momento la información de la tarjeta llegara a pasar o ser almacenada en tu servidor, tendrías que hacerte responsable por cualquiera de las pautas y/o auditorías de PCI DSS.

Culqi proporciona las siguientes formas de tokenizar la información de pago de tu cliente de manera segura bajo HTTPS, para integrarte debes elegir cualquiera de ellas:

- [Culqi Checkout](#culqi-checkout) (Recomendada, la más simple)
- [Culqi.js](#culqijs) (Checkout personalizado)
- [Móviles](#moviles-ios-y-android) (Android e iOS)

> Para usar Culqi Checkout o Culqi JS debes tener instalado JQuery.

<br/>

### Culqi Checkout

La manera más simple de empezar la integración con Culqi.


<br/><img src="/devsite/img/pagos/checkout.png"/><br/>

Culqi Checkout es un formulario embebido que facilita la integración de Culqi en tu sitio web y móvil (adaptable, soporta tablets y smartphones). Además, te permite personalizar el título y subtítulo a mostrar. Para integrarlo debes seguir los siguientes pasos:

#### 1. Incluir Culqi Checkout para tokenizar la tarjeta

Para la tokenizar los datos de la tarjeta de forma segura, primero incluye el script de Culqi Checkout en tu página. Puedes incluirlo antes del cierre de la etiqueta `body`:

```
<script src="https://checkout.culqi.com/js/v3"></script>
```

> El .js del checkout siempre debe ser incluido desde https://checkout.culqi.com/js/v3 . Usar una copia local puede resultar en errores visibles para el usuario.

Posteriorment, en otra etiqueta, debajo del llamado al Culqi Checkout, configura tu llave pública:

<!--DOCUSAURUS_CODE_TABS-->
<!--JavaScript-->
```javascript
Culqi.publicKey = 'Aquí inserta tu llave pública';
```
<!--END_DOCUSAURUS_CODE_TABS-->

Tu llave pública nos permite identificar las comunicaciones entre tu cliente (sitio web) y Culqi. Recuerda reemplazarlo con tu propia llave pública del ambiente de integración.

> Para obtener tu llave pública ingresa a la sección Desarrollo > API Keys ubicada en el Panel de Culqi.

Por ultimo Configura los valores a mostrar en el formulario de Culqi Checkout.

<!--DOCUSAURUS_CODE_TABS-->
<!--JavaScript-->
```javascript
Culqi.settings({
    title: 'Culqi Store',
    currency: 'PEN',
    description: 'Polo/remera Culqi lover',
    amount: 3500
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

#### 2. Muestra el Checkout de Culqi
El segundo paso es mostrar el formulario de Culqi. En este paso el cliente ingresa la información de su tarjeta, y el formulario lo convierte en un token.

<!--DOCUSAURUS_CODE_TABS-->
<!--JavaScript-->
```javascript
$('#buyButton').on('click', function(e) {
    Culqi.open();
    e.preventDefault();
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

#### 3. Enviar el ID del objeto token hacia tu servidor

Luego de haberse generado el token, este estará disponible en la función `culqi()`. Una vez que ha sido obtenido lo debes de enviar a tu servidor como mínimo el Id del token(token.id) para ser procesado y crear un cargo o una suscripción.

<!--DOCUSAURUS_CODE_TABS-->
<!--JavaScript-->
```javascript
function culqi() {
  if (Culqi.token) { // ¡Objeto Token creado exitosamente!
      var token = Culqi.token.id;
      alert('Se ha creado un token:' + token);
  } else { // ¡Hubo algún problema! 
      console.log(Culqi.error);
      alert(Culqi.error.user_message);
  }
};
```
<!--END_DOCUSAURUS_CODE_TABS-->

<br/>

### Culqi JS

Si quieres brindarles a tus clientes una experiencia personalizada de compra, entonces debes de usar Culqi JS.

<br/><img src="/devsite/img/pagos/custom-checkout.png"/><br/>



Culqi JS facilita la integración de Culqi en tu sitio web y móvil. Además,te permite personalizar todo el formulario de Culqi, para integrarlo debes seguir los siguientes pasos:

#### 1. Desarrolla tu checkout personalizado con Culqi JS

El primer paso es crear el formulario de captura de la tarjeta. En este paso el cliente ingresa la información de su tarjeta, para que Culqi JS pueda crear el objeto Token usando la función `Culqi.createToken()`.

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


#### 2. Integra Culqi JS y crea un objeto Token

Para tokenizar los datos de la tarjeta de forma segura, primero incluye el JS en tu página. Puedes incluirlo antes del cierre de la etiqueta "body" o dentro de la etiqueta "head":

```html
<script src="https://checkout.culqi.com/v2"></script>
```

En otra etiqueta, debajo del llamado al JS, configura tu llave pública:

<!--DOCUSAURUS_CODE_TABS-->
<!--JavaScript-->
```javascript
Culqi.publicKey = 'Aquí inserta tu llave pública';
Culqi.init();
```
<!--END_DOCUSAURUS_CODE_TABS-->

Tu llave pública nos permite identificar las comunicaciones entre tu cliente (sitio web) y Culqi. Recuerda reemplazarlo con tu propia llave pública del ambiente de integración.

> Para obtener tu llave pública ingresa a la sección Desarrollo > API Keys ubicada en el Panel de Culqi.

<!--DOCUSAURUS_CODE_TABS-->
<!--JavaScript-->
```javascript
$('#btn_pagar').on('click', function(e) {
    Culqi.createToken();
    e.preventDefault();
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

#### 3: Enviar el token hacia tu servidor

Luego de haberse generado el token, este estará disponible en la función culqi(). Una vez que ha sido obtenido lo debes de enviar a tu servidor como mínimo el Id del token(token.id) para ser procesado y crear un cargo o una suscripción.


<!--DOCUSAURUS_CODE_TABS-->
<!--JavaScript-->
```javascript
function culqi() {
  if (Culqi.token) { // ¡Objeto Token creado exitosamente!
      var token = Culqi.token.id;
      alert('Se ha creado un token:' + token);
  } else { // ¡Hubo algún problema! 
      console.log(Culqi.error);
      alert(Culqi.error.user_message);
  }
};
```
<!--END_DOCUSAURUS_CODE_TABS-->

<br/>

### Culqi Android/iOS

Uno de las formas más rápidas y fáciles de tokenizar los datos de tarjeta desde una aplicación móvil es con Culqi Android o Culqi iOS, para integrarlo debes seguir los siguientes pasos:

#### 1. Instalar dependencias

Para tokenizar los datos de la tarjeta de forma segura desde un dispositivo Android, primero debes instalarlo en tu proyecto. Puedes aprender a hacerlo aquí.

#### 2. Crear un Token

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
<!--Objetive C-->
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
