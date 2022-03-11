# Automatizaciones varias

Automatizaciones que no se pueden englobar en ningún sitio, pero son igual de útiles

## Saber si un Shelly tiene actualización

¿Te gustaría tener tu Shelly con la última actualización por parte del fabricante?, voy mirando uno a uno, hasta que vea si tiene actualizaciones o no, esta automatización te avisará vía telegram de que tienes una actualización en tu dispositivo:

```yaml

alias: Actualización Zigbee disponible
description: >-
  Avisa por Telegram cuando hay una actualización disponible para un dispositivo
  Zigbee
trigger:
  - platform: mqtt
    topic: zigbee2mqtt/bridge/log
condition:
  - condition: template
    value_template: >-
      {{ trigger.payload_json.type == "ota_update" and
      trigger.payload_json.meta.status == "available"}}
action:
  - service: notify.telegram_casa
    data:
      message: >-
        📡 Actualización de firmware z2m disponible para {{
        trigger.payload_json.meta.device | replace("_","-") }}
mode: parallel
max: 25

```
:fontawesome-brands-telegram:{ .telegram } <small>@serMayoral</small> 