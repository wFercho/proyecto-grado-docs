# Generación de alertas

Link: [Link a este design doc](#)

Author(s): Fernando Salamanca

Status: Draft

Última actualización: YYYY-MM-DD

## Contenido

- Goals
- Non-Goals
- Background
- Overview
- Detailed Design
  - Componentes
    - Alerts Service
    - Rules Service
    - Notification Service
    - Event Bus
    - MQTT Broker
  - Flujo de Datos
- Consideraciones
- Métricas

## Links

- [Un link](#)
- [Otro link](#)

## Objetivo

Este documento describe el diseño del sistema de alertas para monitoreo en minas subterráneas. El sistema permite detectar condiciones anómalas en tiempo real a partir de datos de sensores IoT y notificar a los usuarios y sistemas interesados.

El sistema se compone de tres microservicios principales:
- **Rules Service**: Define y gestiona reglas de monitoreo.
- **Alerts Service**: Evalúa los datos en tiempo real y genera alertas.
- **Notification Service**: Maneja la distribución de alertas a clientes web, móviles y otros sistemas.

## Goals

- Detectar condiciones peligrosas en tiempo real.
- Permitir la definición flexible de reglas para distintas variables y ubicaciones.
- Notificar eficientemente a usuarios y sistemas interesados.
- Garantizar la escalabilidad y resiliencia del sistema.

## Non-Goals

- No se busca almacenar datos históricos de sensores (esto es manejado por otro servicio de almacenamiento).
- No se encarga de la visualización de datos (esto es responsabilidad de la aplicación frontend).

## Background

Las minas subterráneas utilizan sensores IoT para monitorear variables como temperatura, gases y vibraciones. Detectar condiciones peligrosas es crítico para la seguridad de los trabajadores y la continuidad de las operaciones.

Este sistema de alertas permite evaluar datos en tiempo real y generar notificaciones automatizadas a través de múltiples canales.

## Overview

El sistema de alertas sigue el siguiente flujo de trabajo:
1. **Sensores IoT** envían datos al **MQTT Broker**.
2. **Alerts Service** consume estos datos, consulta reglas en el **Rules Service** y evalúa si se debe generar una alerta.
3. Si se genera una alerta:
   - Se almacena en la base de datos de **Alerts Service**.
   - Se publica en el **Event Bus**.
4. **Notification Service** recibe alertas del **Event Bus** y las envía a clientes web/móviles mediante WebSockets o Push Notifications.
5. Otros sistemas interesados pueden suscribirse al **Event Bus** para recibir alertas directamente.

## Detailed Design

### Componentes

#### 1. Alerts Service

**Responsabilidades:**
- Suscribirse a los tópicos del **MQTT Broker** para recibir datos de sensores.
- Consultar el **Rules Service** para cargar reglas y poder evaluar si un dato activa una alerta.
- Persistir alertas, a modo de registro, en su base de datos.
- Publicar alertas en el **Event Bus**.

**API Endpoints:**
- `POST /alerts` → Crear alerta (interno).
- `GET /alerts` → Listar alertas.
- `GET /alerts/{id}` → Obtener detalles de una alerta.

**Modelo de Datos:**
```json
{
  "id": "alert-123",
  "variable": "temperatura",
  "valor": 35,
  "categoria": "PELIGRO",
  "ubicacion": "mina_1/tunel_3",
  "timestamp": "YYYY-MM-DDTHH:MM:SSZ"
}
```

#### 2. Rules Service

**Responsabilidades:**
- Permitir la creación, actualización y consulta de reglas de monitoreo.
- Definir umbrales cualitativos para cada variable (ej. temperatura: **NORMAL, ADVERTENCIA, PELIGRO**).
- Exponer reglas mediante una API para que **Alerts Service** pueda consultarlas.

**API Endpoints:**
- `POST /rules` → Crear regla.
- `GET /rules` → Listar reglas.
- `GET /rules/{id}` → Obtener una regla específica.
- `PUT /rules/{id}` → Actualizar una regla.
- `DELETE /rules/{id}` → Eliminar una regla.

**Modelo de Datos:**
```json
{
  "id": "rule-456",
  "variable": "temperatura",
  "scope": {
    "level": "zona",
    "id": "zona-123"
  },
  "thresholds": [
    { "category": "NORMAL", "min": 10, "max": 20 },
    { "category": "ADVERTENCIA", "min": 21, "max": 30 },
    { "category": "PELIGRO", "min": 31, "max": 100 }
  ]
}
```

#### 3. Notification Service

**Responsabilidades:**
- Recibir alertas desde el **Event Bus**.
- Distribuir notificaciones en tiempo real a clientes web/móviles mediante WebSockets o Push Notifications.

**API Endpoints:**
- `GET /notifications` → Listar notificaciones enviadas.

#### 4. Event Bus

**Responsabilidades:**
- Permitir que **Alerts Service** publique alertas.
- Permitir que **Notification Service** y otros sistemas las consuman.

Opciones: Kafka, RabbitMQ, AWS SQS, NATS.

#### 5. MQTT Broker

**Responsabilidades:**
- Recibir datos de sensores IoT.
- Permitir que **Alerts Service** se suscriba a tópicos de sensores.

Opciones: Eclipse Mosquitto, EMQX, HiveMQ.

### Flujo de Datos

```plaintext
[IoT Sensor] → [MQTT Broker] → [Alerts Service] → [Event Bus] → [Notification Service] → [Web/Mobile]
                                ↳ [Database]
                                ↳ [Otros Servicios]
```

## Consideraciones

- **Escalabilidad:** El sistema debe manejar miles de sensores enviando datos en tiempo real.
- **Seguridad:** Se debe restringir el acceso a las APIs mediante autenticación y autorización.
- **Tolerancia a fallos:** Uso de colas de mensajes y almacenamiento persistente para evitar pérdida de datos.

## Métricas

- **Tiempo de respuesta del Alerts Service.**
- **Cantidad de alertas generadas por día.**
- **Cantidad de notificaciones enviadas por Notification Service.**
- **Tasa de consumo de eventos en Event Bus.**


