# Bienvenido

Bienvenido a la Wiki del [**Canal de Telegram Home Assistan Fácil**](https://t.me/DomoticaFacilHomeAssistant), aquí encontrarás todo tipo de tutoriales, ayudas y recetas que se han hablado en el grupo.

Puedes aportar tus ideas e incluso hacer tus propios tutoriasles, mas información aquí.


## ❤️ Si quieres aportar

¿No encuentras la información que buscas?, ¿te gustaría hacer algún tutorial?, ¿la información no es correcta?, puedes hacerlo de las siguientes maneras:

* Avisarnos a través del [canal de Telegram](https://t.me/DomoticaFacilHomeAssistant)
* Poner una petición en [la página Github del proyecto](https://github.com/ConsolaViejuna/mkdocs-homeAssistantFacil)

## Preguntas frecuentes

**¿Cuál es el mejor mini ordenador para Home Assistant?**

Raspberry Pi 4 de 4GB, mejor relación precio-prestaciones, suficiente para instalar Home Assistant, además de que podrás usarla para más cosas, Kodi, Plex (sin transcodificación), servidor web casero, etc...
Ultimamente ha subido mucho de precio, por lo que siempre se puede valorar el uso de un mini ordenador o NUC.

**¿Necesito un ventilador para la Raspberry Pi 4?**

Si, pero hazte con uno de calidad, también puedes usar un buen disipador.

**¿Como conecto mi servidor de Home Assistant, por wifi o por cable?**

Siempre que puedas por cable.

**¿Tarjeta Micro SD o disco duro?**

De momento puedes empezar con una tarjeta SD de calidad, recomendas, las Samsung Evo de clase 10, en un futuro valora poner un disco duro (SSD o normal) por USB, tarde o temprano las tarjetas SD se corrompen (no están preparadas para ser escritas tantas veces como un disco duro).

**¿Que es eso del Zigbee?**

Es un protocolo de comunicación que usa tecnología Wifi para comunicarse con los dispositivos domóticos, la ventaja de esta red es que es independiente de la Wifi de casa, usa tecnología de malla y su consumo de energía es ínfimo.

**¿Que necesito para tener Zigbee?**

Disponer de un router zigbee y algún dispositivo domótico zigbee

Routers:

| Nombre      | Características                      | Precio | Observaciones | Enlace |
| :---------- | :----------------------------------- |:-------| :------------ |:-------|
| CC2531      |  Hasta un máximo de 30 dispositvos, no soporta dispositivos Zigbee 3.0 | 17€ | Para empezar en este mundillo, aunque se puede quedar corto | <a href="https://es.aliexpress.com/item/4000439478555.html?spm=a2g0o.productlist.0.0.391f1e35oSctFh&algo_pvid=74662e2a-7366-4871-8960-f0e6a4fcc796&algo_exp_id=74662e2a-7366-4871-8960-f0e6a4fcc796-1&pdp_ext_f=%7B%22sku_id%22%3A%2210000001807961687%22%7D" target="_blank">:link:</a> |
| ZZH     |  Hasta un máximo de 300 dispositvos.  | 45€ | El ideal, :warning: ojo que viene desde Inglaterra a varios compañeros les están cobrando aduanas pese a venir con el IVA pagado |<a href="https://www.tindie.com/products/electrolama/zzh-cc2652r-multiprotocol-rf-stick/" target="_blank">:link:</a> |
| Sonoff 3.0     |  Funcionamiento muy parecido al ZZH  | 14€ | Según comentan varios compañeros funciona perfectamente, viene con IVA ya aplicado | <a href="https://itead.cc/product/sonoff-zigbee-3-0-usb-dongle-plus/" target="_blank">:link:</a> |                     

**¿Que es Tasmota?**

Es un firmware alternativo para dispositivos basados en el chip ESP8266 que nos permite controlarlos desde una interfaz web y estos son compatibles con Home Assistant

**¿Si quiero poner tasmota en mis dispositivos, tengo que soldar?**

La mayoría de veces sí, también puedes usar pinzas o algo para hacer conexión, además de un programador TTL, no es difícil, solo es echarle un rato y disfrutar

** ¿Es mi dispositivo compatible con Tasmota? **

Puedes mirarlo <a href="https://templates.blakadder.com/" target="_blank">aquí.</a> 

** ¿Es mi dispositivo compatible zon Zigbee? **

Puedes mirarlo <a href="https://zigbee.blakadder.com/" target="_blank">aquí.</a> 

** Busco una caja para mi Raspberry **

Sin duda esta es la ideal, <a href="https://es.aliexpress.com/item/4000243184501.html?spm=a2g0o.productlist.0.0.65355a89QIauYV&algo_pvid=97adce66-26fa-4886-b6c3-428fd6eb527b&algo_exp_id=97adce66-26fa-4886-b6c3-428fd6eb527b-10&pdp_ext_f=%7B%22sku_id%22%3A%2212000025708638957%22%7D" target="_blank">Argon Pi</a> , dispone de ventilación pasiva y ventilador regulable en velocidad. Revisa que la caja tenga los mismos conectores que tu Raspberry ya que hay varios modelos. <a href="./integraciones/argonOne/" target="_blank">Si quieres saber más.</a> 

** ¿Que altavoz multimedia elijo? **

Según las experiencias de los miembros del grupo los altavoces Google son más fáciles de integrar que Alexa, no obstante Alexa se puede integrar perfectamente con un poquito de trabajo :man_factory_worker:

** ¿Que temperatura debe de tener mi Raspberry? ** 

Aunque aguanta perfectamente una temperatura de trabajo de hasta 80ºC, para un funcionamiento estable y duradero os recomendamos una temperatura de 50ºC.

** ¿Me recomiendas un SAI? **

Válido para un Router, una raspberry y una ONT, <a href="https://www.amazon.es/dp/B07W8MCBMS/ref=cm_sw_r_u_apa_fabc_UGP4FbNT572QA?_encoding=UTF8&th=1" target="_blank">este</a> .Te recomendamos que uses una <a href="https://es.aliexpress.com/item/32844394210.html?srcSns=org.telegram.messenger&spreadType=socialShare&bizType=ProductDetail&social_params=60004530086&tt=MG&aff_platform=default&sk=_BSSzN0&aff_trace_key=227ba6ff7c744c40be1e0e6779cef341-1608720507373-06473-_BSSzN0&shareId=60004530086&businessType=ProductDetail&platform=AE&terminal_id=792811bbc90645a2a7418cd147152d92" target="_blank">cable corto</a> para alimentar tu Raspberry.






