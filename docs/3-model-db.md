# Modelo de base de datos de Abasto

## Diseño

El modelo representa los productos, compradores, pedidos e inventario de cada distribuidora, junto con los empleados autorizados y las acciones que realizan. La interpretación se apoya también en la [definición del proyecto](definition.md).

El diagrama conceptual tiene **ocho entidades**: seis con identificador propio y dos que dependen de otras para identificarse (`EMPLOYEE` e `INVENTORY_BALANCE`). Las identificaciones de la tabla se obtienen de los atributos y las relaciones del diagrama.

| Entidad                                     | Identificación              | Qué representa y qué datos guarda                                                                                                                                                                                                |
| ------------------------------------------- | --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Empresa (`COMPANY`)                         | `company_id`                | Distribuidora que utiliza Abasto. Guarda su nombre (`name`).                                                                                                                                                                     |
| Empleado (`EMPLOYEE`)                       | `(company_id, employee_id)` | Persona autorizada dentro de una empresa. Guarda las referencias de identidad (`sender_identity`, `subject_identity`), su estado (`state`) y sus permisos (`permissions`).                                                       |
| Comprador (`BUYER`)                         | `buyer_id`                  | Restaurante atendido por una distribuidora. Guarda su nombre (`name`).                                                                                                                                                           |
| Producto (`PRODUCT`)                        | `product_id`                | Referencia del catálogo de una distribuidora. Guarda su nombre (`name`) y unidad de manejo (`unit`), por ejemplo, cajas o bolsas.                                                                                                |
| Pedido (`ORDER`)                            | `order_id`                  | Compra solicitada por un comprador y registrada por un empleado. Guarda su estado (`status`).                                                                                                                                    |
| Saldo de inventario (`INVENTORY_BALANCE`)   | `product_id`                | Existencias de un producto: físicas (`stock_on_hand`), reservadas (`reserved_quantity`) y disponibles (`available_quantity`). Las dos últimas se calculan.                                                                       |
| Operación de pedido (`ORDER_OPERATION`)     | `operation_id`              | Solicitud de confirmar, cancelar o despachar. Guarda la clave de reenvío (`idempotency_key`), el tipo (`type`), la huella del contenido (`request_hash`), el resultado (`result`) y la respuesta original (`original_response`). |
| Movimiento de inventario (`STOCK_MOVEMENT`) | `movement_id`               | Cambio en las existencias físicas. Guarda el tipo (`type`), la cantidad que se suma o resta (`quantity_delta`) y el motivo (`reason`).                                                                                           |

Las cardinalidades `(mínimo, máximo)` indican cuántos vínculos puede tener cada instancia de la entidad señalada; `N` permite varios.

| Relación                                  | Participación de cada entidad        | Interpretación                                                                                                                                               |
| ----------------------------------------- | ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Empresa autoriza empleado                 | Empresa `(0,N)`; empleado `(1,1)`    | Una empresa puede tener varios empleados autorizados; cada registro de empleado pertenece a una empresa.                                                     |
| Empresa atiende comprador                 | Empresa `(0,N)`; comprador `(1,1)`   | Cada comprador pertenece a una distribuidora; esta puede tener varios compradores.                                                                           |
| Empresa cataloga producto                 | Empresa `(0,N)`; producto `(1,1)`    | Cada producto pertenece al catálogo de una distribuidora; esta puede tener varios productos.                                                                 |
| Empleado registra pedido                  | Empleado `(0,N)`; pedido `(1,1)`     | Cada pedido lo registra un empleado; un empleado puede registrar varios pedidos.                                                                             |
| Comprador solicita pedido                 | Comprador `(0,N)`; pedido `(1,1)`    | Cada pedido corresponde a un comprador; un comprador puede tener varios pedidos.                                                                             |
| Pedido incluye producto (`INCLUDES`, N:M) | Pedido `(1,N)`; producto `(0,N)`     | Todo pedido incluye al menos un producto; un producto puede aparecer en varios pedidos o en ninguno. La relación guarda la cantidad solicitada (`quantity`). |
| Producto tiene saldo                      | Producto `(1,1)`; saldo `(1,1)`      | Cada producto tiene exactamente un saldo, y cada saldo corresponde a un producto.                                                                            |
| Empleado ejecuta operación                | Empleado `(0,N)`; operación `(1,1)`  | Cada operación la ejecuta un empleado; un empleado puede ejecutar varias.                                                                                    |
| Empleado registra movimiento              | Empleado `(0,N)`; movimiento `(1,1)` | Cada movimiento lo registra un empleado; un empleado puede registrar varios.                                                                                 |
| Pedido documenta operación                | Pedido `(1,N)`; operación `(0,1)`    | Un pedido puede tener al menos una operación; una operación puede estar vinculada a un pedido o a ninguno.                                                   |
| Pedido origina movimiento                 | Pedido `(0,N)`; movimiento `(0,1)`   | Un pedido puede originar varios movimientos; un movimiento puede estar vinculado a un pedido o a ninguno.                                                    |
| Saldo tiene movimientos                   | Saldo `(0,N)`; movimiento `(1,1)`    | Un saldo puede tener varios movimientos; cada movimiento afecta al saldo de un producto.                                                                     |

### Decisiones e interpretación

- **Datos separados por empresa.** Compradores, productos y empleados pertenecen a una distribuidora. Los pedidos, operaciones y movimientos se vinculan a ella mediante sus relaciones. Todos los participantes de una misma acción deben corresponder a esa empresa.
- **Empleado identificado dentro de su empresa.** `employee_id` es una clave parcial: puede repetirse entre distribuidoras, pero la combinación `(company_id, employee_id)` identifica un único empleado autorizado. Los permisos se asignan por acción y se representan como un atributo multivaluado, porque un empleado puede tener varios.
- **Productos de un pedido.** `INCLUDES` es una relación N:M entre `ORDER` y `PRODUCT`. Cada combinación pedido–producto aparece una sola vez y tiene su propia cantidad (`quantity`), que debe ser positiva y válida para la unidad del producto. La cantidad pertenece al vínculo: el mismo producto puede pedirse en cantidades distintas en cada pedido.
- **Saldo dependiente.** `INVENTORY_BALANCE` comparte la identificación de su producto y existe una sola vez por producto. No tiene identificador propio.
- **Reservas calculadas.** `reserved_quantity` suma las cantidades de ese producto en pedidos confirmados (`CONFIRMED`). `available_quantity = stock_on_hand − reserved_quantity`. Las reservas nunca deben superar las existencias físicas; las entradas y los ajustes deben conservar esa regla. Si dos pedidos compiten por la última unidad, solo uno puede confirmarse.
- **Pedido completo.** Registrar y confirmar ocurre en un solo paso, sin borradores: se crea el pedido con todos sus productos, cantidades y su reserva completa. Si se rechaza la solicitud, se conserva el resultado de la operación sin crear el pedido ni reservar mercancía. Esto explica que una operación pueda existir sin pedido.
- **Cancelación y despacho.** Cancelar un pedido confirmado libera toda la reserva sin cambiar las existencias físicas. Despacharlo descuenta toda su mercancía y cierra la reserva. El pedido termina como cancelado o despachado; cada cambio debe completarse junto con sus efectos sobre el inventario.
- **Reenvíos sin efectos duplicados.** La combinación `(company_id, idempotency_key)` identifica una solicitud dentro de la empresa. `request_hash` permite comprobar que su contenido coincide. Reenviar una operación resuelta conserva su resultado y su respuesta original, incluso si fue rechazada o el pedido cambió de estado. Una solicitud distinta o un nuevo intento tras un rechazo requiere otra clave. El resultado de la operación se consulta por separado del estado actual del pedido.
- **Historial de existencias físicas.** Los movimientos registran existencias iniciales, entradas, ajustes y salidas por despacho, con la cantidad que se suma o resta y el empleado responsable. Los ajustes requieren un motivo. Un movimiento puede existir sin pedido, como una entrada de mercancía. Confirmar y cancelar modifican las reservas, sin generar cambios físicos de mercancía.

Por ejemplo, con diez cajas físicas, confirmar un pedido de dos deja **diez físicas, dos reservadas y ocho disponibles**. Si se cancela, vuelven a quedar diez disponibles; si se despacha, quedan ocho físicas, ninguna reservada y ocho disponibles.

### Modelo conceptual (Chen)

[Modelo conceptual en notación Chen extendida](diagrams/chen-model-db.drawio).
