# Automatizaciones de electrodomésticos

Todo tipo de automatizaciones relacionadas con los electrodomésticos, que te servirán para saber si tu lavadora o tu lavavajillas ha terminado, ¡levántate del sofá!. Estas automatizaciones normalmente se ubican en el fichero **automations.yaml**

## La lavadora ha finalizado

Automatización que te avisa vía Telegram y altavoz multimedia de que la lavadora ha terminado de lavar. Si la lavadora marca por debajo de 4W de consumo te avisará.

??? tip "Requisitos"

    * El enchufe que donde tengas la lavadora, debe tener un medidor de energía.
    * Un notificador configurado, en esta caso    <a href="https://www.home-assistant.io/integrations/telegram/" target="_blank">** Telegram **</a>
    * Un altavoz multimedia

```yaml
- id: 'Lavadora Finalizada'
  alias: Lavadora finalizada
  description: Manda un aviso cuando la lavadora ha terminado
  trigger:
    - below: '4'
      entity_id: sensor.bw_shp2_enchufe_power
      platform: numeric_state
  condition:
    - condition: template
      value_template: '{{ trigger.from_state.state |int(-1) > trigger.to_state.state |int(0) }}'
  action:
    - data:
        message: 🧺 La lavadora ha terminado
      service: notify.telegram_casa
    - data:
        entity_id: media_player.gh_todos
        message: La lavadora ha terminado su programa
      service: python_script.enviar_tts_media
    - delay: '00:00:30'
  mode: single
```
:fontawesome-brands-telegram:{ .telegram } <small> @sermayoral</small> 

## La lavadora ha finalizado (Avanzado)

Automatización que te avisa vía Telegram y altavoz multimedia de que la lavadora ha terminado de lavar. En esta automatización se informa del estado en el que está la lavadora, además detectara si estás en casa o no, para avisarte.

??? tip "Requisitos"

    * El enchufe que donde tengas la lavadora, debe tener un medidor de energía.
    * Un altavoz multimedia

Deberás de crearte un ayudante timer, en el fichero timer.yaml:

```yaml
fin_lavadora:
  duration: '00:15:00'
```
También deberás crearte un input-select para saber en que estado esta tú lavadora, en el fichero input_select.yaml:

```yaml
lavadora_status:
  name: Estado de la lavadora
  options:
   - "Apagado"
   - "Lavando"
   - "Espera"
   - "Finalizado"
  initial: "Apagado"
  icon: mdi:washing-machine
```

Y la automatización (por revisar): 

```yaml
- id: '1611617826606'
  alias: Lavadora Inicio
  description: ''
  trigger:
  - platform: numeric_state
    entity_id: sensor.lavadora_current_consumption
    above: '10'
  condition:
  - condition: state
    entity_id: input_select.lavadora_status
    state: Apagado
  action:
  - service: input_select.select_option
    data:
      entity_id: imput_select.lavadora_status"
      option: Lavando
    entity_id: input_select.lavadora_status
  - service: tts.google_say
    data:
      entity_id: media_player.salon
      language: es
      message: La lavadora está en marcha, te avisaré cuando acabe, para que saques
        la ropa.
  mode: single
- id: '1611618424857'
  alias: Lavadora modo Espera
  description: ''
  trigger:
  - platform: numeric_state
    entity_id: sensor.lavadora_current_consumption
    for: 00:02:00
    below: '2'
  condition:
  - condition: state
    entity_id: input_select.lavadora_status
    state: Lavando
  - condition: state
    entity_id: group.familia
    state: not_home
  action:
  - service: input_select.select_option
    data:
      entity_id: input_select.lavadora_status
      option: Espera
    entity_id: input_select.lavadora_status
  - service: notify.telegram_grupo
    data:
      message: 👚🧥💦 La lavadora ya ha terminado, pasará al modo de espera, cuando llegues
        a casa te lo recordaré
  mode: single
- id: '1611619172762'
  alias: Lavadora Fin de Espera
  description: ''
  trigger:
  - platform: state
    entity_id: person.javi_lopez_alarcon
    to: home
  - platform: state
    entity_id: person.esther_vidal
    to: home
  condition:
  - condition: state
    entity_id: input_select.lavadora_status
    state: Espera
  action:
  - service: input_select.select_option
    data:
      entity_id: imput_select.lavadora_status
      option: Finalizado
    entity_id: input_select.lavadora_status
  mode: single
- id: '1611619633715'
  alias: Lavadora Finalizado
  description: ''
  trigger:
  - platform: numeric_state
    entity_id: sensor.lavadora_current_consumption
    for: 00:02:00
    below: '2'
  condition:
  - condition: or
    conditions:
    - condition: state
      entity_id: person.jaid: '1611617826606'
  alias: Lavadora Inicio
  description: ''
  trigger:
  - platform: numeric_state
    entity_id: sensor.lavadora_current_consumption
    above: '10'
  condition:
  - condition: state
    entity_id: input_select.lavadora_status
    state: Apagado
  action:
  - service: input_select.select_option
    data:
      entity_id: imput_select.lavadora_status"
      option: Lavando
    entity_id: input_select.lavadora_status
  - service: tts.google_say
    data:
      entity_id: media_player.salon
      language: es
      message: La lavadora está en marcha, te avisaré cuando acabe, para que saques
        la ropa.
  mode: single
- id: '1611618424857'
  alias: Lavadora modo Espera
  description: ''
  trigger:
  - platform: numeric_state
    entity_id: sensor.lavadora_current_consumption
    for: 00:02:00
    below: '2'
  condition:
  - condition: state
    entity_id: input_select.lavadora_status
    state: Lavando
  - condition: state
    entity_id: group.familia
    state: not_home
  action:
  - service: input_select.select_option
    data:
      entity_id: input_select.lavadora_status
      option: Espera
    entity_id: input_select.lavadora_status
  - service: notify.telegram_grupo
    data:
      message: 👚🧥💦 La lavadora ya ha terminado, pasará al modo de espera, cuando llegues
        a casa te lo recordaré
  mode: single
- id: '1611619172762'
  alias: Lavadora Fin de Espera
  description: ''
  trigger:
  - platform: state
    entity_id: person.javi_lopez_alarcon
    to: home
  - platform: state
    entity_id: person.esther_vidal
    to: home
  condition:
  - condition: state
    entity_id: input_select.lavadora_status
    state: Espera
  action:
  - service: input_select.select_option
    data:
      entity_id: imput_select.lavadora_status
      option: Finalizado
    entity_id: input_select.lavadora_status
  mode: single
- id: '1611619633715'
  alias: Lavadora Finalizado
  description: ''
  trigger:
  - platform: numeric_state
    entity_id: sensor.lavadora_current_consumption
    for: 00:02:00
    below: '2'
  condition:
  - condition: or
    conditions:
    - condition: statevi_lopez_alarcon
      state: home
    - condition: state
      entity_id: person.esther_vidal
      state: home
  - condition: state
    entity_id: input_select.lavadora_status
    state: Lavando
  action:
  - service: input_select.select_option
    data:
      entity_id: input_select.lavadora_status
      option: Finalizado
    entity_id: input_select.lavadora_status
  mode: single
- id: '1611620171155'
  alias: Lavadora recordarorio
  description: ''
  trigger:
  - platform: state
    entity_id: input_select.lavadora_status
    to: Finalizado
  - platform: event
    event_type: timer.finished
    event_data:
      entity_id: timer.fin_lavadora
  condition: []
  action:
  - service: timer.start
    data:
      duration: '0'
    target:
      entity_id: timer.fin_lavadora
  - service: tts.google_say
    data:
      entity_id: media_player.salon
      language: es
      message: La lavadora ya terminó, debes poner la secadora, te lo recordaré cada
        15 minutos, hasta que me digas, ya he sacado la lavadora
  mode: single
```
:fontawesome-brands-telegram:{ .telegram } <small>@JaviLopezFotografia</small> 


