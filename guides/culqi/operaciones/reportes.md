---
id: reportes
title: Reportes
---

Toda la información de tu cuenta Culqi es accesible a través del Panel de Culqi y API. Adicionalmente, si eres un desarrollador o trabajas con uno, podrás crear reportes customizados a tus necesidades que proveerán mayor flexibilidad en la información que quieras incluir. En esta guía te explicaremos cómo crear reportes personalizados.

## A través del Panel

El Panel de Culqi provee acceso a información en tiempo real acerca de la actividad de tu cuenta en Culqi, el cuál podrás descargar como reportes en formato CSV en la sección Ventas.

## A través del API

Sin embargo, a veces es necesario contar con información y reportes más específicos, mas allá de los que está disponible en el Panel. A través del API de Culqi podrás crear tus propias herramientas de reportes y acceder a toda tu información en tiempo real. Esto provee mucho mayor flexibilidad sobre qué información debería ser incluida y cuándo deberían ser creados este tipo de reportes.

La primera parte de esta guía creará un reporte de pagos utilizando el API muy similar al reporte que está disponible actualmente en el Panel. El resto de la guía explicará como crear algunos reportes de ejemplo. Todos los reportes serán creados en formato CSV y pueden ser abiertos en cualquier hoja de cálculo.

### Crear un reporte de pagos

Nuestro Panel está construido sobre la base del API Culqi, y cada uno de los reportes creados realizan peticiones al mismo API que son procesadas por detrás. Podrás replicar y construir por encima de esta funcionalidad creando tus propias peticiones al API de Culqi. Para comenzar, la siguiente petición al API devuelve un listado de 50 objetos cargos.

```php
<?php
    $charges = $culqi->Charges->getList(array("limit" => 1));
?>
```

Los cargos son encontrados dentro del atributo data y son devueltos en orden cronológico descendente por lo que el cargo más reciente se encuentra primero:

```php
<?php
    $charges->data
?>
```

La información retornada es la base del reporte de pago, así que el siguiente paso es formatear la respuesta en una estructura que funcione bien con el propósito del reporte. Puedes realizar esto expandiendo la petición al API e iterando a través de cada cargo obtenido, identificar la información que quieres incluir y luego extraerlos en el formato que desees. Este ejemplo en PHP script hace lo anteriormente descrito, realizando las siguientes acciones:

- Crea un archivo CSV que contiene la información necesaria en el header
- Realiza una petición al API de los 50 cargos más recientes
- Iterar a través de cada objeto cargo obtenido para identificar el id, description, creation_date, amount y currency_code
- Salvar la información del cargo en un archivo CSV.

```php
<?php

require 'vendor/autoload.php';

use Culqi\Culqi;

$SECRET_API_KEY = '{LLAVE SECRETA}';
$culqi = new Culqi(array('api_key' => $SECRET_API_KEY));

$csv = fopen('cargos.csv', 'w');

$headers = array(
    'Charge ID',
    'Amount',
    'Description',
    'Date',
    'Response Message'
);

fputcsv($csv, $headers);

$charges = $culqi->Charges->getList(array("limit" => 50));

foreach ($charges->data as $charge) {

    $id = $charge->id;
    $amount = $charge->amount/100;
    $description = $charge->description;
    $created = gmdate('Y-m-d H:i', ($charge->creation_date)/1000);
    $message = $charge->outcome->merchant_message;

    $report = array(
        $id,
        $amount,
        $description,
        $created,
        $message
    );

    fputcsv($csv, $report);
}

fclose($csv);
?>
```

El resultado será un archivo CSV que es una versión básica del reporte de pagos que pueden descargar a través del Panel. Con un poco más de desarrollo, puede reemplazar al reporte del Panel de manera completa, permitiéndote obtener el control de toda la data que deseas formatear e interpretar.

### Sugerencias

Existen algunos cambios que nos gustaría recomendarte para que puedas mejorar la creación del reporte:

Hacer uso del parámetro status del objeto Charge para realizar consultas de transacciones exitosas
Incluir un timestamp dentro del nombre del archivo CSV para que un reporte existente no sea reescrito.
En vez de guardar la información del cargo directamente en un archivo CSV, guárdalo en tu base de datos o pásalo directamente a un sistema de reportes automatizados.

### Límite de Listados

El número máximo de objetos que pueden ser devueltos cuando utilizas el parámetro límites es de 100. Para los casos que necesites realizar consultas mayores a este número, es necesario que pagines la petición pasando el cargo final devuelto con el parámetro starting_after en la siguiente petición para listar la siguiente lista de cargos.

```
<?php
    $charges = $culqi->Charges->getList(array("limit" => 100, "after" => "chr_test_7VUwCneoG1XtLeS7"));
?>
```

### Reportes de cargos personalizados

La petición al API para listar una lista de objetos puede incluir filtros adicionales que refinen la data que deseas recibir. Por ejemplo, puedes editar en la petición y consultar por el rango de los montos, lo que hará que devuelvas una lista de cargos asociados al cliente.

```php
<?php
    $charges = $culqi->Charges->getList(array("min_amount" => 500, "max_amount" => 1000,"limit" => 50));
?>
```

### Reportes de tarjetas de débito y crédito

Puede ser útil que entiendas el tipo de tarjeta que está utilizando tu cliente a la hora de realizar una compra. Para esto, podrás determinar el tipo de tarjeta que fue utilizada a la hora de realizar un cargo dándole una mirada al parámetro type del hash source localizado en cada cargo.

```bash
curl -X GET --header 'Accept: as?country_code=PE&limit=50'authorization: Bearer sk_test_UTCQSGcXW8bCyU59' 'https://api.culqi.com/v2/iins
```

### Reportes de devoluciones

Un reporte de cargos siempre es últil para mantener el seguimiento de los cargos que son realizados a través de Culqi. Para tener seguimiento de las devoluciones, un reporte de devoluciones puede ser creado que liste todas las devoluciones realizadas en un rango de tiempo específico. Adicionalmente, este reporte puede incluir información acerca del cargo en cuestión.

```bash
curl -X GET --header 'Accept: application/json' --header 'authorization: Bearer sk_test_UTCQSGcXW8bCyU59' 'https://api.culqi.com/api/v2/refunds?limit=50'
```

La petición al API devuelva un listado de los objetos devoluciones, que luego es iterado a través de la herramienta de reportes. Los datos devueltos están filtrados basados en la fecha de creación de la devolución y otra petición del API es realizada durante cada iteración para consultar la información relacionada al cargo. Toda esta información es traída toda junta para proveer un reporte de devoluciones solicitadas en un rango de tiempo determinado con información del cargo al que está relacionado.

### Más usos de los reportes personalizados

El foco de este ejemplo fue realizar reportes de cargos y devoluciones, sin embargo puedes crear los reportes que imagines utilizando peticiones similares al API para cualquier información, incluyendo:

- Tokens
- Cargos
- Tarjetas
- Transferencias
- Clientes
- Planes
- Suscripciones
- Eventos