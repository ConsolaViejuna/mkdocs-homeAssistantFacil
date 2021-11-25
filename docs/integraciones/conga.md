# Conga

El aspirador "barato" made in spain, pero que al fin y al cabo es un diseño totalmente chino, la competencia barata de la Roomba, actualmente no está soportado por Home Assistant, pero se puede integrar a través de <a href="https://github.com/txitxo0/congatudo-add-on" target="_blank">**Congatudo**</a>, todo ello gracias al trabajo de los chicos de <a href="https://freecon.ga/" target="_blank">**FreeConga.**</a>

<figure markdown> 
  ![codigoComentado](img/conga.jpg)
  <figcaption>Una conga sacando brillo</figcaption>
</figure>

## Modelos soportados

Actualmente estos son todos los modelos soportados:

* Todos los modelos 3X90
* 4090
* 5490

## Proceso de instalación

Lo primer de todo vamos a instalar este Addon que permite manejar la Conga a nuestro antojo.

**Instalando Congatudo**

Añade el siguiente repositorio desde aquí:

[![Open your Home Assistant instance and show the add add-on repository dialog with a specific repository URL pre-filled.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https://github.com/txitxo0/congatudo-add-on)

Ve al menú **Supervisor :material-arrow-right: Tienda de Complementos** y busca el addon Congatudo:

<figure markdown> 
  ![codigoComentado](img/congatudo.jpg){ width="300" }
</figure>

Haz click en instalar, una vez instalado, inicia el addon con Start.

**Instalado AddGuard**

Si no tienes este maravilloso addon,instala el addon addGuard, para ello ve al menú **Supervisor :material-arrow-right: Tienda de Complementos** y busca el addon AddGuard y procedes a instalarlo.

<figure markdown> 
  ![codigoComentado](img/addGuard.jpg){ width="300" }
</figure>

Ahora vamos crear unas reglas de filtrado, para que toda petición que haga la Conga a sus servidores (que están en china), la haga directamente al addon de Congatudo, para ello selecciona la opción **Filters :material-arrow-right: DNS Rewrites** selecciona la opción DNS Rewrite y añades las siguientes entradas:

Entrada 1

* **Domain:** *.3irobotics.net
* **IP:** Tu dirección IP local donde tienes instalado Home Assistant

Entrada 2

* **Domain:** *.3iroboticx.net
* **IP:** Tu dirección IP local donde tienes instalado Home Assistant

Te debería de quedar algo así:

<figure markdown> 
  ![codigoComentado](img/dns.jpg)
</figure>

**Hacer que nuestra...

En construcción

