# Scripts para estar mejor

Un script, es un conjunto de acciones que se van a ejecutar secuencialmente, aquí te ponemos las más interesantes que hemos recopilado del grupo. Estos scripts normalmente se ubican en el fichero **scripts.yaml**

## Abrir ventanas dependiendo de la humedad exterior

Si vives en una ciudad costera o con alta humedad, aqui hay un script para comparar la humedad exterior e interior de tu vivienda, para que te aconseje si abrir o no las ventanas de casa para ventilar. Como es un script, deberás lanzarlo, por ejemplo cuando abras las ventanas.

??? tip "Requisitos"

    * Un altavoz multimedia configurado, que admita tts.
    * Sensores de humedad interior y exterior.

```yaml
puedo_abrir_las_ventanas:
  sequence:
  - service: tts.google_say
    data:
      entity_id: media_player.salon
      message: '"{% if (states("sensor.humedad_media_casa") | int) < (states("sensor.humedad_exterior_matrimonio")
        | int) -%} Yo no abriría. Hay un {{ (states("sensor.humedad_exterior_matrimonio")
        | int)-(states("sensor.humedad_media_casa")| int)}} Por ciento mas de Humedad
        en el exterior  {%- elif (states("sensor.humedad_media_casa")  | int) > (states("sensor.humedad_exterior_matrimonio")
        | int) %}    Puedes abrir. Hay un {{ (states("sensor.humedad_media_casa")
        | int) -(states("sensor.humedad_exterior_matrimonio") | int) }} Por ciento
        menos de Humedad en el exterior  {%- else -%}  Puedes abrir, pero no tardes
        en cerrar. La humedad ahora en el exterior es similar al interior  {%- endif
        %}"'
  mode: single
  alias: Puedo Abrir las Ventanas
  icon: mdi:window-closed-variant
```

**Variables usadas**

| Variables                   | Tipo       | Descripción                         |
| ----------------------------| -----------|-------------------------------------|
| `sensor.humedad_media_casa` | *sensor* | Humedad media de la casa, se calcula a partir de varios sensores, [más información.](http://localhost:8000/automatizaciones/confort/#media-de-temperatura-o-humedad)  |
| `sensor.humedad_exterior_matrimonio` | *sensor* | Sensor exterior de humedad  |
| `tts.google_say` | *servicio* | Servicio para que el altavoz multimedia diga la frase escrita a través de tts  |

:fontawesome-brands-telegram:{ .telegram } <small>@JaviLopezFotografia</small> 