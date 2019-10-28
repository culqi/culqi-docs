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