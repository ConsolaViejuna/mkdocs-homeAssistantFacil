# Habilitar sonido en tu Host con Home Assistant Supervised instalado

Para todos aquellos que tengáis instalado Home Assistant Supervised sobre Debian/Raspberry Pi OS, y quieran utilizar el sonido desde el Sistema Operativo del Host (por ejemplo para [Kodi](https://kodi.tv)), es posible que hayan observado que el sonido no funciona. Si te ocurre lo mismo, no te preocupes que vamos a poder darle una solución.

Esto es debido a que el Supervisor se apodera del controlador de audio del Host, de tal forma que el Host pierde el control del audio y, por consiguiente, Kodi o el reproductor que uses en el Host no tendrá sonido.

## Buscando una solución

La verdad es que no fue nada fácil dar con la tecla, primeramente porque tampoco teníamos claro por qué no teníamos sonido. Podían ser mil cosas: Kodi, el propio Raspberry Pi OS 64 (que estaba en una versión beta cuando lo instalamos), etcétera. No obstante, tras mucho probar, tocar e investigar, finalmente vimos que el sonido se rompía justo después de instalar Home Assistant, así que ya teníamos bastante acotado el problema.

Ya solo tuve que navegar un poco para dar con el señor [shanness](https://github.com/shanness) (muy majo, por cierto) y su pedazo de [tutorial](https://github.com/shanness/HomeAssistant-PulseAudio-Disable), en el que explica por qué pasa, dando una solución al problema. La verdad es que lo explica de forma muy técnica, y por eso he decidido facilitarlo un poco haciendo este tutorial.

La solución al problema consiste en decirle al contenedor de audio que usa el supervisor, llamado *hassio-audio*, que libere el controlador de audio del Host si no lo está usando. Es una solución muy sencilla y limpia, pues la mayoría del tiempo Home Assistant no usa el audio del Host. Esto lo conseguiremos con una orden que le daremos al contenedor. ¿Cual es el problema? Que los contenedores, por su propia filosofía, pierden todos los cambios que hagamos sobre ellos cuando se reinicie o se actualice (a no ser que el propio contenedor se construya con dicha directiva, pero como no es un contenedor nuestro que podamos editar, pues descartamos esa opción).

Lo bueno de este problema es que nos va a permitir aplicar la solución de manera temporal para verificar si se corrige nuestro problema de sonido. ¿Que lo corrige? Pues montamos toda la parafernalia que hizo nuestro amigo *shanness* para hacerlo "definitivo". ¿Qué no funciona? Pues descartamos esta solución y tendríamos que buscar por otro lado ya que, como os dije antes, que el sonido no funcione puede ser por mil cosas (problemas de drivers, hardware, etc.).

## Verificar que nuestro problema de sonido es el descrito

Para ello tan solo tenemos que ejecutar la siguiente orden desde el Sistema Operativo de nuestro Host:

```
docker exec -it hassio_audio pactl load-module module-suspend-on-idle
```

Con esto habremos activado el módulo *suspend-on-idle* en el controlador de audio del contenedor *hassio_audio*. Podemos verificar que lo hemos cargado correctamente ejecutando el siguiente comando:

```
docker exec -it hassio_audio pactl list modules
```

En la que se nos listará todos los módulos cargados en el controlador de audio del contenedor *hassio_audio*:

```

Module #0
        Name: module-device-restore
        Argument:
        Usage counter: n/a
        Properties:
                module.author = "Lennart Poettering"
                module.description = "Automatically restore the volume/mute state of devices"
                module.version = "14.2"

Module #1
        Name: module-stream-restore
        Argument:
        Usage counter: n/a
        Properties:
                module.author = "Lennart Poettering"
                module.description = "Automatically restore the volume/mute/device state of streams"
                module.version = "14.2"

...

Module #14
        Name: module-position-event-sounds
        Argument:
        Usage counter: n/a
        Properties:
                module.author = "Lennart Poettering"
                module.description = "Position event sounds between L and R depending on the position on screen of the widget triggering them."
                module.version = "14.2"

Module #15
        Name: module-suspend-on-idle
        Argument:
        Usage counter: n/a
        Properties:
                module.author = "Lennart Poettering"
                module.description = "When a sink/source is idle for too long, suspend it"
                module.version = "14.2"
```

En mi caso podéis observar que sí se ha cargado correctamente como módulo #15.

Ahora tan solo tenéis que verificar que el sonido ya funciona en vuestro sistema Host. En mi caso ejecuté Kodi y... voilà. ¡Volvía a escuchar sonido!

## Haciendo el cambio permanente

En el caso de que la solución descrita os haya funcionado, como os dije anteriormente, solo se mantendrá hasta que el contenedor se reinicie o se actualice, momento en el que dejaréis de tener sonido de nuevo. ¿Cómo lo hacemos permanente? Muy fácil, vamos a definir un servicio que se encargue de estar monitorizando el contenedor *hassio_audio* para que, cuando pierda el módulo *suspend-on-idle*, lo vuelva a cargar. Para ello tenemos que buscar un directorio dentro de nuestra carpeta de usuario en donde alojar el script. En mi caso he decidido alojarlo dentro de una nueva carpeta *Scripts* que cuelgue directamente de mi carpeta de usuario:

```
cd ~ && mkdir Scripts && cd Scripts
```

Ahora os váis a descargar todo el repositorio de nuestro amigo *shanness* con [git](https://git-scm.com/). Considero que es bastante mejor hacerlo con *git* que descargando y descomprimiendo, porque si nuestro amigo sube cambios/mejoras, podréis actualizaros tan solo ejecutando un comando, y no repitiendo todo el proceso. Dicho esto, procedemos a la clonación del repositorio:

```
git clone https://github.com/shanness/HomeAssistant-PulseAudio-Disable.git
```

Una vez clonado el repositorio, entramos dentro de la carpeta que se ha creado y podemos ver los dos ficheros que nos interesa: *pa-suspend.sh* (el script) y *pa-suspend.service* (el servicio).

Lo primero que vamos a hacer es darle permisos de ejecución al script con el siguiente comando: 

```
chmod +x pa-suspend.sh
```

Después vamos a editar el servicio. Para ello tenemos que indicarle dos parámetros:

1. User: El usuario de nuestro sistema del Host (en mi caso **pi**)
2. ExecStart: La ruta absoluta de la ubicación del script.

Un consejo para obtener la ruta absoluta de un directorio donde estemos ubicado es ejecutar el siguiente comando:

```
pwd
```

Y esto os devolverá la ruta completa del directorio donde estéis situados actualmente, que en mi caso es */home/pi/Scripts/HomeAssistant-PulseAudio-Disable*. Si a esta ruta le añadís una / y el nombre del script, ya tenéis la ruta absoluta de vuestro script, que en mi caso es */home/pi/Scripts/HomeAssistant-PulseAudio-Disable/pa-suspend.sh*

Pues bien, ahora ya podéis editar el fichero *pa-suspend.service* (el servicio) para añadirle los dos datos que hemos mencionado. Podéis editarlos con el editor que más os guste. En mi caso he usado *nano*:

```
nano pa-suspend.service
```

El *User* no lo tuve que modificar porque coincidía con el mío, pero sí que tuve que editar la ruta absoluta de la ubicación del script, quedándome de la siguiente manera:

```
[Unit]
Description=Loads PulseAudio suspend-on-idle module in hassio_audio when started
After=docker.service
Before=hassio-supervisor.service

[Service]
Type=simple
User=pi
ExecStart=/home/pi/Scripts/HomeAssistant-PulseAudio-Disable/pa-suspend.sh
Nice=19

[Install]
WantedBy=multi-user.target
```

Una vez editado, guardamos los cambios y vamos a copiarla en la ruta donde se guardan todos los servicios, con el siguiente comando:

```
sudo cp pa-suspend.service /etc/systemd/system
```

Ahora creamos el servicio *systemd* con el siguiente comando:

```
sudo systemctl enable pa-suspend
```

Y por último lo arrancamos con el siguiente comando:

```
sudo systemctl start pa-suspend
```

## Verificación

Vamos a comprobar que todo lo hemos hecho de manera correcta. Lo primero es ver que el servicio está arrancado y funcionando, y lo haremos con el siguiente comando:

```
sudo systemctl status pa-suspend
```

Tenéis que comprobar que está activo (running). Acto seguido vamos a comprobar que el script *pa-suspend* está ejecutándosa con el siguiente comando:

```
ps -ef | grep pa-suspend
```

Deben aparecer, por lo menos, estos dos procesos:

```
pi          1015       1  0 mar12 ?        00:00:00 /bin/bash /home/pi/Scripts/HomeAssistant-PulseAudio-Disable/pa-suspend.sh
pi          1122    1015  0 mar12 ?        00:00:00 /bin/bash /home/pi/Scripts/HomeAssistant-PulseAudio-Disable/pa-suspend.sh
```

Por último vamos a comprobar que no haya errores en la ejecución del script, con el siguiente comando:

```
sudo journalctl -u pa-suspend
```

En mi caso se muestra que todo ha ido bien, y que el módulo se ha cargado correctamente (Succeeded):

```
-- Journal begins at Fri 2022-01-28 04:21:52 CET, ends at Sun 2022-03-13 03:03:02 CET. --
mar 10 02:45:53 raspberrypi4 systemd[1]: Started Loads PulseAudio suspend-on-idle module in hassio_audio when started.
mar 10 02:45:53 raspberrypi4 pi[10478]: pa-suspend.sh: Started
mar 10 03:10:06 raspberrypi4 systemd[1]: Stopping Loads PulseAudio suspend-on-idle module in hassio_audio when started...
mar 10 03:10:06 raspberrypi4 systemd[1]: pa-suspend.service: Succeeded.
mar 10 03:10:06 raspberrypi4 systemd[1]: Stopped Loads PulseAudio suspend-on-idle module in hassio_audio when started.
```

Es posible que a vosotros se os muestre un error como el siguiente:

```
Mar  5 01:11:38 RPiHost pi: pa-suspend.sh started
Mar  5 01:11:39 RPiHost pi: pa-suspend.sh (Script Start): PulseAudio module-suspend-on-idle failed to load! (Failure: Module initialization failed)
```

Este error es debido a que no lo puede cargar porque ya está cargado (el que cargásteis para la prueba). No es problemático, y lo importante es que el servicio ya está funcionando y se encargará de mantener el módulo cargado siempre. A mi no se me mostró el error porque reincié el sistema antes de arrancar el servicio, pero no tenéis que hacerlo.