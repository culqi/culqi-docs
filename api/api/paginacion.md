---
id: paginacion
title: Paginación
---

Todos los recursos principales de Culqi soportan operaciones de listado, entre ellos Tokens, Cargos, Transferencias, Tarjetas, Clientes, Suscripciones, Devoluciones y Planes. Adicionalmente, todos los métodos de listado del API comparten una estructura similar tomando en cuenta estos tres parámetros: limit, after y before.

Culqi utiliza una paginación en tiempo real basada en cursor a través de los parámetros after y before. Un cursor se refiere a un string de caracteres random que marca un ítem específico en una lista de datos. A menos que el ítem sea borrado, el cursor siempre estará punteando la misma parte de la lista, pero será invalidado si el ítem es removido.

En Culqi, los parámetros after y before toman el valor ID y retornan los objetos en un orden cronológico reverso. El parámetro before devuelve los objetos creados antes del objeto en cuestión. El parámetro after devuelve los objetos creados después del objeto en cuestión. Si ambos parámetros son provistos sólo before es utilizado.

----

## Ejemplo de Paginación

En el siguiente ejemplo podrás conocer cómo realizar una petición para obtener un listado de los últimos 50 cargos.

**Petición**