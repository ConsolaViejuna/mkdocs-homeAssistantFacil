# Automatizaciones de energía

Todo tipo de automatizaciones relacionadas con la energía, que te servirán para detectar si se ha ido la luz, si ha terminado un lavavajillas o tienes el te terminado.

## Detectar que se ha ido la luz

Automatización para detectar que se ha ido la luz, ya sea por un corte de corriente o avería, una vez detectado el corte manda un mensaje a Telegram. Tu router y tu raspberry deberá de estar conectada a un sistema de alimentación interrumpida (SAI).

??? tip "Requisitos"

    * Un medidor de energía, normalmente un PZEM.
    * Un notificador configurado, en esta caso    <a href="https://www.home-assistant.io/integrations/telegram/" target="_blank">** Telegram **</a>
   

```yaml
alias: Comprobar electricidad cortada
description: >-
  Detecta si la luz se ha ido, bien por exceder el consumo máximo permitido, o
  simplemente por haberse producido una avería eléctrica en el barrio. También
  comprueba si se ha reestablecido
trigger:
  - platform: state
    entity_id: sensor.general_consumo_instantaneo
    for: '00:00:10'
action:
  - choose:
      - conditions:
          - condition: template
            value_template: '{{ trigger.from_state.state | float > 3300 }}'
          - condition: template
            value_template: '{{ trigger.to_state.state == "unavailable" }}'
        sequence:
          - service: notify.telegram_casa
            data:
              message: >-
                ⚡️ La luz se ha ido en la vivienda, probablemente por exceso de
                potencia contratada, ya que se estaban consumiendo {{
                trigger.from_state.state }} W/h
          - service: var.set
            data:
              entity_id: var.bool_no_hay_electricidad
              value: true
      - conditions:
          - condition: template
            value_template: '{{ trigger.from_state.state | float <= 3300 }}'
          - condition: template
            value_template: '{{ trigger.to_state.state == "unavailable" }}'
        sequence:
          - service: notify.telegram_casa
            data:
              message: >-
                ⚡️ La luz se ha ido en la vivienda, probablemente por una avería
                en la zona
          - service: var.set
            data:
              entity_id: var.bool_no_hay_electricidad
              value: true
      - conditions:
          - condition: template
            value_template: '{{ trigger.from_state.state == "unavailable" }}'
          - condition: template
            value_template: '{{ trigger.to_state.state != "unavailable" }}'
          - condition: template
            value_template: '{{ is_state("var.bool_no_hay_electricidad", "True") }}'
        sequence:
          - service: notify.telegram_casa
            data:
              message: ⚡️ El suministro de electricidad se ha reestablecido
          - service: var.set
            data:
              entity_id: var.bool_no_hay_electricidad
              value: false
mode: single
```