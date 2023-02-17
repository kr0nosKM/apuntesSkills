TAGS: #DHCP #services #utilities #dnsmasq 

Como sabemos Dnsmasq también es un servicio DHCP, el cual ofrece IPs con un rango en específco entre muchísimas otras opciones. De hecho esta utilidad tiene gran cantidad de este tipo de opciones.

## CAMPOS IMPORTANTES:

`dhcp-range=<DIRECCIÓN INICIAL>,<DIRECCIÓN FINAL>,<DURACIÓN>`: Esta es una de las más importantes ya que le estamos indicando el rango específico con una duración específica, la duración da un poco igual en casos de pruebas y tal, yo por defecto le dejo 12h.

`interface=<NOMBRE INTERFAZ>`: Esta opción también es importante, porque le estamos indicando porque interfaz escuchará. (También se puede utilizar la opción de `listen-address=<IP ADDRESS>` para limitar un poco y no indicar toda la interfaz, pero en mi caso no hará falta.)

Existen muchas mas opciones interesantes, pero por ahora dejará únicamente las esenciales.

## OPCIONES COMUNES PARA EL CAMPO `dhcp-option:<OPCIÓN>,<VALOR>`

`dnsmasq --help dhcp`: Para mostraar todas las opciones que tiene dnsmasq sobre DHCP.

Luego estas opciones se tienen que indicar de una forma específica, con el parámetro `dhcp-option=option:<OPCIÓN>,<VALOR>` _(esta opción tiene muchos más parámetros opcionales que introducir, pero los más básicos son los que he mostrado)_

> Los nombres de las opciones también se pueden poner en formato decimal según el estándar __RFC2132__ 
>> __ENLACE (Apartado 3 en adelante):__ https://www.rfc-editor.org/rfc/rfc2132

___Lista completa de parámetros para la opción `dhcp-option=`__:
`dhcp-option=[tag:<tag>,[tag:<tag>,]][encap:<opt>,][vi-encap:<enterprise>,][vendor:[<vendor-class>],][<opt>|option:<opt-name>|option6:<opt>|option6:<opt-name>],[<value>[,<value>]]`



Cabe recalcar que si no le introducimos las siguientes opciones a dnsmasq, por defecto tomará como router(gateway) y dns-server el servidor donde está corriendo, en mi caso daría igual si se pone o no porque el servidor se encarga de todo, pero en algunos entornos en los que hubiese un enrutador diferente y el dnsmasq estuviese en otro servidor, se tendría que indicar o sinó no funcionaría.

`dhcp-option=option:router, 192.168.13.1`
`dhcp-option=option:dns-server, 192.168.13.1`

__FICHERO DE OPCIONES__:

Para mejorar la gestión de las opciones del DHCP, se puede crear un fichero para guardar la configuración, en este fichero se introduciría igual que como en el fichero de configuración. Se puede hacer de diferentes formas, las principales serían, utilizar la opción de `dhcp-optsfile=<RUTA>` (esta es para indicarle específicamente las opciones para el DHCP) o también `conf-file=<RUTA>` (esta opción es para directamente indicar más opciones de todo tipo)

En mi caso utilizaré la opción de `dhcp-optsfile=/etc/dnsmasq.d/<FICHERO>` . Importante decir que por defecto donde se almacenan los ficheros de configuración es en el directorio `/etc/dnsmasq.d/`, en le caso de que se desee cambiar eso, se tendría que modificar el fichero `/etc/default/dnsmasq`.

