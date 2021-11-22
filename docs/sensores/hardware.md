# Sensores para monitorizar tu hardware

¿Se calienta tu Raspberry Pi?, ¿mucho poco?, y el ordenador que tienes en casa, ¿cuanto disco le queda?, todos eso y más podrás monitorizarlo desde Home Assistant:

## Sensor para saber cuando tiempo lleva encendida tu Raspberry

Edita el fichero configuration.yaml, y añade este código en la parte de sensores.

```yaml
sensor:
  # System Monitor
  - platform: systemmonitor
    scan_interval: 60 
    resources:
      - type: processor_use
      - type: memory_use_percent
      - type: last_boot
      - type: swap_use_percent
      - type: disk_use_percent
        arg: /home
  # Uptime sensor (tiempo desde el ultimo reinicio de HA)
  - platform: uptime
  # Sensores aplantillados
  - platform: template
    sensors:
      uptime_templated:
        icon_template: mdi:clock-start
        value_template: >-
          {% set diferencia = ((now().timestamp()- as_timestamp(states('sensor.uptime')))/60) | int %}
          {% set minutes = (diferencia % 60) | int %}
          {% set hours = ((diferencia % 1440) / 60) | int %}
          {% set days = (diferencia / 1440) | int %}
          {{ days }}d{{ ' : ' }}{{ hours }}h{{ ' : ' }}{{ minutes }}m
      uptimerpi_templated:
        icon_template: mdi:clock-fast
        value_template: >-
          {% set diferencia = ((now().timestamp()- as_timestamp(states('sensor.last_boot')))/60) | int %}
          {% set minutes = (diferencia % 60) | int %}
          {% set hours = ((diferencia % 1440) / 60) | int %}
          {% set days = (diferencia / 1440) | int %}
          {{ days }}d{{ ' : ' }}{{ hours }}h{{ ' : ' }}{{ minutes }}m
```
