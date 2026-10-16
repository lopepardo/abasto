# Abasto: software para distribuidoras de alimentos e insumos

Abasto es una aplicación para distribuidoras que venden alimentos e insumos a restaurantes. Permite a sus empleados gestionar productos, controlar el inventario y atender los pedidos desde su registro hasta su cancelación o despacho. Su objetivo es que ventas y bodega trabajen con información confiable sobre la mercancía y los compromisos con sus compradores.

Cada distribuidora cuenta con su propio catálogo, existencias, compradores, pedidos y usuarios. Los empleados autorizados de una empresa pueden trabajar con su información, sin acceder a la de otras distribuidoras. Dentro de esa empresa, los permisos se asignan por acción, como confirmar pedidos, cancelar, despachar o ajustar existencias.

Los restaurantes son los compradores de cada distribuidora. Se comunican con los vendedores para hacer sus pedidos, y estos los registran y gestionan en Abasto.

La aplicación busca resolver situaciones como estas:

- Dos vendedores prometen la misma mercancía porque ambos creen que todavía está disponible.
- Una persona vuelve a enviar un pedido porque la confirmación tarda y termina registrándolo dos veces.
- Ventas y bodega trabajan con cantidades desactualizadas porque las entradas, reservas y salidas de mercancía se llevan en registros separados.

El catálogo permite identificar los productos que vende la distribuidora y la unidad en que se manejan, por ejemplo, cajas o bolsas. El inventario muestra cuánta mercancía hay físicamente en bodega, cuánta está reservada y cuánta sigue disponible. Los empleados registran las existencias iniciales, las entradas de mercancía y los ajustes necesarios, indicando el motivo de cada ajuste. Una corrección no puede reducir las existencias por debajo de lo que ya está reservado.

Un pedido puede incluir uno o varios productos. Se registra y confirma en un solo paso, sin conservar borradores. Confirmarlo significa que toda la mercancía solicitada queda apartada para ese comprador. Si alguno de los productos no tiene suficientes existencias o una cantidad no es válida, la operación se rechaza sin crear el pedido ni apartar mercancía.

Por ejemplo, si hay diez cajas disponibles y se confirma un pedido de dos, quedan dos reservadas y ocho disponibles para otras compras. Las cajas reservadas siguen físicamente en bodega hasta su despacho.

El ciclo del pedido permite:

- **Confirmar:** reservar la mercancía y asumir el compromiso con el comprador.
- **Cancelar antes del despacho:** liberar toda la reserva para que la mercancía vuelva a estar disponible, sin cambiar las existencias físicas.
- **Despachar:** registrar la salida de toda la mercancía del pedido, descontarla de las existencias físicas y cerrar su reserva.

Si dos compras compiten por la última caja, solo una puede confirmarse. Cada solicitud para registrar y confirmar un pedido, cancelarlo o despacharlo se identifica como una operación. Reenviar una operación ya resuelta conserva su resultado original, tanto si tuvo éxito como si fue rechazada, sin volver a modificar el inventario.

Por ejemplo, una operación rechazada por falta de existencias conserva ese rechazo aunque después ingrese mercancía. Intentar la compra de nuevo requiere una operación nueva. El resultado original de una operación y el estado actual del pedido se consultan por separado.

Si una confirmación tarda o su respuesta no llega, los empleados pueden consultar el resultado de esa operación antes de registrar otra compra. Así evitan confundir una operación pendiente de verificar con un rechazo confirmado.

Las personas que utilizan Abasto son:

- **Ventas:** para registrar pedidos, consultar su estado y prometer entregas respaldadas por mercancía reservada.
- **Bodega:** para registrar entradas y ajustes, consultar las reservas, preparar pedidos y registrar sus despachos.
- **El responsable de la distribuidora:** para mantener el catálogo y los compradores, y conocer el estado de los pedidos y del inventario.

Estas tareas describen a los participantes del proceso. Los permisos asignados a cada empleado determinan qué acciones puede realizar.
