---
id: tarjetas-de-prueba
title: Tarjetas de Prueba
---

Culqi te proporciona las siguientes tarjetas de prueba para que puedas validar tu integración y ralizar el mejor desarollo posible.

- Tarjetas exitosas
- Tarjetas con errores específicos

## Tarjetas exitosas

Tarjetas reales de crédito o débito no pueden ser usadas para realizar pruebas en el entorno de integración de Culqi. Usa cualquiera de estas tarjetas para realizar un cargo exitoso.

| Marca            | Número              | Mes/Año | CVV  | Descripción   |
|------------------|---------------------|---------|------|---------------|
| Visa             | 4111 1111 1111 1111 | 09/2020 | 123  | Venta exitosa |
| Mastercard       | 5111 1111 1111 1118 | 06/2020 | 039  | Venta exitosa |
| American Express | 3712 1212 1212 122  | 11/2020 | 2841 | Venta exitosa |
| Diners Club      | 360012 1212 1210    | 04/2020 | 964  | Venta exitosa |

## Tarjetas con errores específicos

Las siguientes tarjetas pueden usarse para crear errores y respuestas específicas, esto es útil para probar en diferentes escenarios y con los códigos de error más comunes.

| Marca            | Número              | Mes/Año | CVV  | Codigos de Denegación    |
|------------------|---------------------|---------|------|--------------------------|
| Visa             | 4000 0200 0000 0000 | 10/2019 | 354  | stolen_card              |
| Visa             | 4000 0300 0000 0009 | 08/2024 | 836  | lost_card                |
| Visa             | 4000 0400 0000 0008 | 03/2021 | 295  | insufficient_funds       |
| Mastercard       | 5400 0000 0000 0005 | 01/2022 | 492  | contact_issuer           |
| Mastercard       | 5400 0200 0000 0003 | 07/2022 | 203  | incorrect_cvv            |
| American Express | 3700 010000 00000   | 04/2021 | 2511 | issuer_not_available     |
| American Express | 3700 020000 00008   | 05/2022 | 1810 | issuer_decline_operation |
| Diners Club      | 3600 000000 0008    | 09/2019 | 683  | invalid_card             |
| Diners Club      | 3600 010000 0007    | 12/2024 | 820  | processing_error         |
| Diners Club      | 3600 020000 0006    | 01/2020 | 230  | fraudulent               |