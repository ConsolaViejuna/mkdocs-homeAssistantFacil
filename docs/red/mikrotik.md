# Mikrotik

¿El router de tu operadora, se ha quedado corto?, ¿tienes problemas con el wifi de tu casa?, ¿y no puedes asignas mas IPs fijas?. Mikrotik es tu respuesta, los routers de la operadora son bastante básico, y para unos poco dispositivos es suficiente, pero que pasa cuando empiezas a tener bastantes dispositivos, tu red se vuelve inmanejable.

Podrás usar un Mikrotik, siempre y cuando puedas deshacerte del router de tu operadora, o puedas poner tu router en modo Bridge, de tal manera que toda la gestión de la red se la pasas al Mikrotik.

!!! warning "A tener en cuenta, la interfaz de usuario de Mikrotik no es nada amigable, y es de nivel avanzado, por lo que no te esperes menús sencillos y asistentes, requiere de paciencia y entender que estás haciendo, desde el grupo te echaremos una mano"

## ¿Que alternativas tengo?

Según las experiencias del grupo, hay varias alternativas:

### Usar un Mikrotik MikroTik Hex RB750Gr3  + AP Unifi

Con esta combinación, podrás el router Mikrotik gestionará tu red, y el [Punto de Acceso Ubitiqui](ubitiqui.md) gestionará toda tu red wifi.

<figure markdown> 
  ![Mikrotik](img/mikrotikHex.jpg)
  <figcaption>Mikrotik Hex Lite</figcaption>
</figure>

Tiene un precio de <a href="https://www.amazon.es/gp/product/B01MSUMVUB/ref=ox_sc_act_title_1?smid=A1CSD37BFGDG5P&psc=1" target="_blank">** 50.87€ **</a>


### Usar un Mikrotik HAP AC2 ###

Con este Router podrás tener todo en uno, por un lado podrás gestionar tu red, y por otro lado la red Wifi, hay que tener en cuenta que el Wifi de Mikrotik no es de lo mejor del mercado en cuanto alcance, por lo que si en la última habitación de tu casa no te llega el Wifi, con el AP AC2 tampoco te va a llegar.

<figure markdown> 
  ![Mikrotik](img/mikrotikAC2.jpg){ width="150" }
  <figcaption>Mikrotik Hex Lite</figcaption>
</figure>

Tiene un precio de <a href="https://www.amazon.es/gp/product/B079SD8NVQ/ref=ox_sc_act_title_1?smid=A2RB1QEU2VOSLR&psc=1" target="_blank">** 62.33€ **</a>

### Usar tu propio Router y una red Zigbee ###

Si aun así no quieres cambiar el Router de tu operadora, puedes usar el mayor número de dispositivos con una red Wifi Zigbee alternativa. 

## Cómo tener tu red local en cualquier

¿Te gustaría poder llevarte tu red local a cuestas?, esto es posible, necesitas de hardware adicional (Hap Mini),para ello sigue el tutorial siguiente realizado por Pocoyo en el foro ADSLAyuda.


<figure markdown> 
  ![Mikrotik](img/hapMini.jpg){ width="200" }
  <figcaption>Hap Mini <a href="https://www.amazon.es/gp/product/B079SD8NVQ/ref=ox_sc_act_title_1?smid=A2RB1QEU2VOSLR&psc=1" target="_blank">21€</figcaption>
</figure>


!!! info "<a href="https://www.adslzone.net/foro/mikrotik.199/manual-mikrotik-tuneles-eoip-como-llevarte-tu-red-cuestas.520975/" target="_blank">** Tutorial **</a>"

## Conecta tu movil Android a tu router

¿Te has quedado sin internet en el router? tienes alternativas, puedes conectar tu móvil Android a tu Router Mikrotik y tener internet, estos son los pasos que has de seguir:

!!! info "<a href="https://blog.ligos.net/2017-08-16/Mikrotik-And-LTE-via-Android.html" target="_blank">** Conectar móvil Android a Mikrotik **</a>"

## Usar dominio de Mikrotik para acceder a Home Assistant (SSL)

Decir primero que el requisito de conocimientos para realizar esto es saber abrir puestos en Mikrotik para el caso del Hairpin NAT (o NAT Loopback). 

Desinstalar o parar el addon de DuckDNS. Ya no vamos a usarlo

Abrir el puerto 80 tcp externo y redireccionarlo al puerto 80 de nuestra host (rpi, nuc o lo que tengais). Este paso es necesario para poder securizar nuestro dominio con el addon Let's Encrypt. Suponiendo que vuestro host esté en la ip 192.168.2.10, el comando para abrir el puerto en Mikrotik sería el siguiente:

```
/ip firewall nat
add action=dst-nat chain=dstnat comment=Hassio-LetsEncrypt dst-address-list=public-ip dst-port=80 protocol=tcp to-addresses=192.168.2.10 to-ports=80
```
Instala el addon de Lets Encrypt desde la Add-on Store del menú Supervisor y configúrarlo de la siguiente manera:

```yaml
email: '!secret email_sergio'
domains:
 - '!secret home_domain'
 certfile: fullchain.pem
 keyfile: privkey.pem
 challenge: http
 dns: {}

```
*!secret home_domain será vuestro DDNS de Mikrotik sin https, que es del tipo blablabla.sn.mynetname.net*

El mail es necesario para poder generar los certificados (lo guarda let's encrypt en su base de datos), de tal manera que cuando  tu certificado esta a punto a finalizar te mandan correo, este addon automatiza la renovación de los certificados. Una vez configurado, ejecutarlo y mirar el log para ver que ha securizado correctamente.

Una vez securizado vuestro dominio, tan solo quedaría especificarlo en el addon de NGINX Proxy. Esta sería la configuracion del addon:

```yaml
domain: '!secret home_domain'
certfile: fullchain.pem
keyfile: privkey.pem
hsts: max-age=31536000; includeSubDomains
cloudflare: false
customize:
  active: false
  default: nginx_proxy_default*.conf
  servers: nginx_proxy/*.conf
```

*Donde secret_home_domain es vuestro DDNS de Mikrotik, es decir, el mismo que configurasteis en el addon de Lets Encrypt*

Una vez modificado, reiniciais el addon, y podríais acceder a vuestra instancia de HA securizada escribiendo: [nombredominio.sn.mynetname.net](nombredominio.sn.mynetname.net)


