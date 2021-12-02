# Automatizaciones para estar mejor

¿Tienes calor?, ¿tienes frío?, uff que pereza, tengo que levantarme a enceder la calefacción, ¿dónde está el mando del aire acondicionado?, ¡Aquí huele a cerrado!, hay que ventilar, pero ¿no hay mucha humedad?, se acabó, con estas automatizaciones te tendrás que olvidar de casi todo. Estas automatizaciones normalmente se ubican en el fichero **automations.yaml**

##Aviso para abrir ventanas dependiendo de la humedad

Indicado para climas húmedos, si las ventanas estan cerradas y fuera hay menos de un 10% de humedad respecto a la de dentro, te aconseja que abras las ventanas, pero si las tienes abiertas y hay mas humedad fuera que dentro, te aconseja que cierres para no elevar la humedad interior. Además te avisa a una hora determinada.

??? tip "Requisitos"

    * Sensores de humedad o temperatura.
    * Altavoz Multimedia Google (con Alexa se podría hacer).

```yaml
- id: '1638122386567'
  alias: Airear la Casa
  description: Realiza una commparativa de la humedad interior y exterior y te aconseja
    si abrir o no las ventanas
  trigger:
  - platform: template
    value_template: '{{ (states("sensor.humedad_exterior_h_matrimonio") | int)<=(states("sensor.humedad_media_casa")
      | int)-10}}'
  - platform: template
    value_template: '{{ (states("sensor.humedad_exterior_h_matrimonio") | int)>=(states("sensor.humedad_media_casa")
      | int)}}'
  - platform: time
    at: '10:30'
  condition:
  - condition: and
    conditions:
    - condition: state
      entity_id: group.familia
      state: home
    - condition: time
      after: '10:00'
      before: '17:00'
  action:
  - choose:
    - conditions:
      - condition: and
        conditions:
        - condition: template
          value_template: "{{ (states(\"sensor.humedad_exterior_h_matrimonio\")\n\
            \        | int)<=(states(\"sensor.humedad_media_casa\")| int)-10}}"
        - condition: state
          entity_id: group.ventanas_aire
          state: 'off'
      sequence:
      - service: tts.google_say
        data:
          entity_id: media_player.todos_gh
          language: es
          message: Podrías aprovechar y abrir las ventanas para ventilar la casa,
            Hay un {{(states("sensor.humedad_media_casa") | int) - (states("sensor.humedad_exterior_h_matrimonio") 
            | int) }} Por ciento menos de Humedad en el exterior.
    - conditions:
      - condition: and
        conditions:
        - condition: template
          value_template: "{{ (states(\"sensor.humedad_exterior_h_matrimonio\")\n\
            \        | int)>=(states(\"sensor.humedad_media_casa\")| int)}}"
        - condition: state
          entity_id: group.ventanas_aire
          state: 'on'
      sequence:
      - service: tts.google_say
        data:
          entity_id: media_player.todos_gh
          message: Deberias de cerrar las ventanas. Hay un {{     (states("sensor.humedad_exterior_h_matrimonio")
            |int) -      (states("sensor.humedad_media_casa")| int)}} Por ciento mas
            de Humedad en el exterior
          language: es
    default: []
  mode: single
```