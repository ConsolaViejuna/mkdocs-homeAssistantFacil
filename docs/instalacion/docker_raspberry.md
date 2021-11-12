# Instalación de Home Assistant sobre Docker en Raspberry

Existe otro método de instalación en Home Assistant, consiste en instalar Docker sobre el sistema operativa Raspbian, y sobre este instalar un contenedor con Home Assistant, esto te permite poder utilizar la Raspberry como una ordenador normal, con su escritorio, utilidades y  además usar Home Assistant.

Para este método de instalación os recomendamos tener disponer al menos de una Raspberry Pi 4 con al menos 4 GB de ram

⚠️ Ojo, este tipo de instalación no está soportada por la comunidad de Home Assistant, ¿esto que significa?:

* Te aparecerá un aviso en tu servidor de que este tipo de instalación no está soportada.
* Si encuentras algún bug o error en la aplicación este no puede ser reportado a la comunidad de Home Assistant

En la práctica somos muchos miembros del grupo los que tenemos este tipo de instalación, y no estamos teniendo ningún tipo de problema, incluso no están atendiendo los bugs que reportamos.

<a href="https://community.home-assistant.io/t/installing-home-assistant-supervised-on-raspberry-pi-os/201836" target="_blank">**✅ Instalación de Docker en Raspbian**</a>

## Instalar Raspbian

Para poder hacer este tipo de instalación debes de instalar Raspbian en tu raspberry, para ello deberás grabar en una tarjeta SD (recomendamos mínimo de 64 GB) la siguiente imagen:

[**✅ Imagen Raspberry OS con escritorio**](https://downloads.raspberrypi.org/raspios_armhf/images/raspios_armhf-2021-11-08/2021-10-30-raspios-bullseye-armhf.zip)
