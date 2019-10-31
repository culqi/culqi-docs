---
id: devoluciones
title: Devoluciones
---

En este artículo aprenderás acerca de cómo solicitar una devolución a un cargo exitoso y conocerás el detrás de escena necesario para que tus clientes se queden tranquilos a la hora que les des una respuesta.

## ¿Cómo puedo realizar la devolución de una venta?

Puedes realizar la devolución de un cargo exitoso a través del Panel y el API de Culqi. En ambos casos podrás solicitar los siguientes tipos de devoluciones:

- **Devolución Parcial:** Deseas devolver un monto menor al monto total del cargo.
- **Devolución Total:** Deseas devolver el monto total del cargo

### A través del API

```php
$culqi->Refunds->create(
  array(
    "amount" => 500,
    "charge_id" => "charge_id",
    "reason" => "fraudulento"
  )
);
```

### A través del Panel

Adicionalmente, las razones por las cuales pueden ser devuelta una venta varían y los motivos pueden ser muy diversas. A continuación, una listado de las principales motivos que puedes seleccionar (o no) a la hora de solicitar una devolución:

- **Duplicada:** La venta está duplicada por lo cual se le ha cargado el mismo monto dos o más veces al mismo cliente.
- **Disputa Perdida:** La venta entró a un proceso de disputa y el comercio no tiene ninguna evidencia que presentar y quiere dar por perdido el caso.
- **Fraudulenta:** El comercio descubre que la venta es de alto riesgo y decide devolver el monto de la venta al dueño de la tarjeta antes de entregar el producto o servicio.
- **Solicitada por el Cliente:** El cliente solicitó que se realice una devolución por mal servicio, producto defectuoso u otro motivo particular.

## Pedí una devolución y todavía no llega a mi cliente. ¿Qué hago?

En caso la devolución la realices el mismo día que se procesó la venta con éxito (p.e. la venta fue el Lunes 1 de Marzo y la Devolución el 1 de Marzo también), la liberación de los fondos a la tarjeta de crédito, débito o pre-pagada del cliente es inmediata y el saldo de tu cliente nunca se ve comprometido ya que los fondos de la compra nunca han salido de la cuenta bancaria de tu cliente.

Es importante que tengas en cuenta que algunos bancos tardan 1 día o más en actualizar sus servicios web y móvil de banca online, por lo que tu cliente podrá seguir viendo el cargo exitoso a pesar que hayas solicitado la devolución con éxito.

En caso la devolución la realices un día después o los días sub-siguientes de haberse procesado la venta éxito (p.e. la venta fue el Lunes 1 de Marzo y la Devolución el 8 de Marzo), el tiempo de devolución de los fondos dependerá de los tiempos que tarde el banco emisor de la tarjeta de tu cliente en conciliar con la marca procesadora. Usualmente la liberación de los fondos se realiza entre 5 a 10 días hábiles después de que hayas solicitado la devolución a Culqi. En caso haya pasado más de este tiempo desde que hiciste la solicitud, es muy importante que nos escribas a culqi.com/soporte para poder ayudarte con el caso.

## ¿Cómo puedo cancelar una devolución?

Las devoluciones no pueden ser canceladas. Si quisieras realizar un cargo por el monto que has devuelto, puedes crear un nuevo cargo para tu cliente.

Si a la tarjeta a la que estás haciendo la devolución está expirada o se encuentra cancelada, los fondos de la devolución se reembolsaran la nueva tarjeta del cliente. En el extraño caso que un cliente no tenga una nueva tarjeta, el banco usualmente enviará los fondos directamente a la cuenta bancaria del cliente.

En el peor de los escenarios (el cliente ha cerrado la cuenta bancaria) el banco no sabrá que hacer y devolverá los fondos completos a Culqi. En este punto, nos contactaremos contigo vía email para preguntarte sobre cómo quieres manejar la devolución.

## ¿Hasta cuánto tiempo puedo solicitar una devolución?

Puedes solicitar una devolución hasta 360 días después de que se haya procesado la compra. Luego de este periodo, los bancos ya no aceptan devoluciones de ninguna venta solicitada por el comercio, por lo que la devolución de los fondos tendrá que ser a través de otro medio.

## ¿Tiene algún costo realizar una devolución?

No. Las devoluciones no tienen ningún costo asociado. Sin embargo, si los fondos de la venta ya se han depositado a tu cuenta bancaria y solicitas una devolución de esa venta, en este caso Culqi no te devolverá la comisión fija cargada por transacción exitosa pero si la comisión variable acorde al monto devuelto.

