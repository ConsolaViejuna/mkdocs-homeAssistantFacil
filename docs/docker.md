# Docker

En esta sección te desvelaremos todos los secretos para manejar docker como un pro, y perderle miedo a los comandos que harán de tu vida con docker una experiencia sencilla.

También te enseñaremos la manera de instalar todos los contendedores que harán de tu ordenador una máquina funcional y preparada para casi cualquier tarea.

## Bloquear las actualizaciones en Docker

Alguna vez, se ha reportado algún error de tal manera que cuando Docker se actualiza, este no es compatible con Home Assistant, por lo que tenemos que esperar a que Docker saque una actualización, si actualizas tu sistema Linux habitualmente y no quieres actualizar Docker hasta estar seguro de que no se produce ningún problema de compatibilidad puedes hacer que Docker no se actualize.

**Para bloquear la actualización:**

```
  sudo apt-mark hold docker-ce sudo apt-mark hold docker-cli-ce
```

**Para desbloquear la actualización:**

```
sudo apt-mark hold docker-ce sudo apt-mark hold docker-cli-ce
```

Si has tenido problemas con la versión Docker 20.10.4, ejecuta los siguientes comandos:

```
sudo nano /usr/sbin/hassio-supervisor
```

Comentamos la última línea con un # y añadimos la siguiente línea:

```
runSupervisor
```