# Abasto

Abasto es una aplicación en construcción para distribuidoras de alimentos e insumos que venden a restaurantes. Busca que ventas y bodega compartan información confiable sobre productos, existencias y pedidos, para evitar comprometer mercancía que ya está reservada.

## Propósito

Este proyecto nació para practicar y profundizar en el desarrollo de software a partir de un problema del mundo real. La idea es recorrer el proceso completo, desde entender el negocio hasta poner una aplicación en funcionamiento, documentando las decisiones y lo aprendido.

Durante ese recorrido se trabajará en:

- Definición de requerimientos e iteración de la idea.
- Análisis del dominio y DDD estratégico.
- Diseño e implementación de la base de datos.
- Desarrollo y pruebas de las reglas del negocio.
- Infraestructura y despliegue en la nube.
- Uso de IA como apoyo para analizar, diseñar y generar código, revisando y comprendiendo sus propuestas.

## Alcance inicial

- Gestionar productos y compradores de cada distribuidora.
- Registrar existencias, entradas y ajustes de inventario.
- Confirmar pedidos reservando toda la mercancía solicitada.
- Cancelar pedidos para liberar reservas o despacharlos para registrar su salida.

La regla central es sencilla: **solo se puede prometer mercancía disponible**. Cada distribuidora debe trabajar con sus propios datos y con empleados autorizados para cada acción.
