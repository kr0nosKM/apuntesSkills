TAGS: #dns #dnsmasq #services #utilities 

CONNECTION WITH: [[DNS-CACHING]]

MAN-PAGE: https://thekelleys.org.uk/dnsmasq/docs/dnsmasq-man.html
BASIC FUNCTIONALITY PAGE: https://geekflare.com/setup-dns-caching-dnsmasq-on-ubuntu/
SECURING DNSMASQ: https://alta3.com/blog/securingdnsmasq

## EXTRA INFO

Este servicio además de ser DNS caché también tiene también utilidades de DHCP.

## COMANDOS:

`--max-ttl=<TIEMPO>`: Indica el máximo TTL ofrecido a los clientes en vede del verdadero (en el caso de que este sea más bajo), pero el verdadero TTL sera también almacenado en el caché.


`--max-cache-ttl=<TIEMPO>` y `--min-cache-ttl=<TIEMPO>` indican el tiempo __Máximo / Mínimo__ del DNS a guardar. Según el creador es recomendable no tocar estos rangos (sobretodo el mínimo) si no sabemos lo que hacemos. Este tipo de opciones lo gestiona automáticamente el dnsmaskq

`-p, --port=<port>`: Se puede indicar el puerto por que el que escucha, pero el de por defecto es 53. Si se deja en 0, se desactivará completamente la función de DNS.

`-i, --interface=<NOMBRE DE LA INTERFAZ>`: Será por la interterfaz que escuchará nuestro caché DNS. Lo ideal sería utilizar la interfaz de la red interna, ya que si tenemos puesta la de la INET externa podríamos llegar a sufrir un DNS poisoning. Es importante indicar la interfaz, o sinó escuchará por todas la interfaces (a no ser que utilicemos la opción de `-I, --except-interface=<NOMBRE INTERFAZ>`, para bloquear por ejemplo la externa.) 

`-a, --listen-address=<IP ADDRESS>`: Indicaremos la ip por la que queramos que escuche. Se puede indicar `-i` y `-a` al mismo tiempo.

`--clear-on-reload`: Lo que hace es que cuadno /etc/resolv.conf es releido o algún servidor en producción viea el DBus, limpiará el caché DNS.

## FICHERO DE CONFIGURACIÓN `/etc/dnsmasq.conf`

_Cabe destacar que la versión larga de los comandos de arriba, por ejemplo `--listen-address=<IP>` quitandole el `--` del principio es lo que se pone en el fichero de configuración_

`cache-size=<NUM>`: Esta opción nos permitirá establecer el tamaño máximo de las dominios que el caché registrará.

`port=<PORT>`: Por defecto será el puerto 53. Nos permitirá indicar por el puerto donde el servicio escuchará.

`interface=<INET NAME>`: Indicaremos porque interfaz queremos que escuche.

`bogus-priv`: Evita que el dominio y el puerto se retrasmita (forwarding). Nunca enviará direcciones en direcciones no enrutadas.

## LOGS DE DNSMASQ

`log-queries`: Es una opción bastante interesante porque muestra todas las peticiones que realizan por parte de cualquier cliente que esté con el DNS del  servidor-router.

`log-facility=/root/dnsmasq.log`: En esta parte se almacenarán todos los logs de dnsmasq relacionados con el DNS caché