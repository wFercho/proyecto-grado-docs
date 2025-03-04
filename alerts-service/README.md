# Documento de Diseño: Microservicio de Alertas
## Problema
Necesitamos una forma de generar alertas con base a reglas definidas.
## Objetivos
- Validar datos de entrada de todos los topics publicados en el MQTT Broker para que, con base a reglas definidas, se puedan generar alertas.
- Permitir a partes interesadas el suscribirse a alertas.
- Almacenar logs para casos en los que se esté validando reglas a topics que no estén generando datos.

## Solución propuesta
Desarrollar un microservicio con las siguientes funcionalidades:
1. Suscripción a topics MQTT con reglas activas
2. Validación de la regla activa para topic de la regla
