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

## Estado actual y próximos pasos

El proyecto está en etapa de definición y diseño. Esta lista reúne la documentación disponible y el trabajo pendiente para llegar a una primera versión funcional:

- [x] [Definición del proyecto](docs/1-definition.md): problema, usuarios y comportamiento esperado.
- [x] [DDD estratégico](docs/2-strategic-ddd.md): lenguaje del negocio, responsabilidades y reglas principales.
- [x] [Modelo conceptual de base de datos](docs/3-model-db.md): entidades, relaciones y decisiones del modelo.
- [x] [Diagrama conceptual](docs/diagrams/chen-model-db.drawio): representación del modelo en formato editable de draw.io.
- [ ] **Diagrama relacional:** traducir el modelo conceptual a tablas, claves y relaciones.
- [ ] **Esquema de base de datos:** definir las tablas, restricciones e índices en PostgreSQL.
- [ ] **Decisiones técnicas:** elegir las tecnologías y definir la estructura de la aplicación, documentando el motivo de cada elección.
- [ ] **Entorno de desarrollo:** preparar la configuración y las instrucciones para ejecutar el proyecto localmente.
- [ ] **Infraestructura y despliegue:** preparar los recursos necesarios y publicar una primera versión en la nube.
- [ ] **Documentación y aprendizaje:** mantener las instrucciones de uso y registrar las decisiones, dificultades y aprendizajes de cada etapa.
