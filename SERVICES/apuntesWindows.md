# CREACIÓN DE UN CONTROLADOR DE DOMINIO

## DNS

El DNS normalmente lo creará automaticamente, pero si tenemos un DNS desmarcaremos la opción y posteriormente le indicaremos el servidor DNS.

## NOMBRE NETBIOS

Si no se pone el nombre de NETBIOS automáticamente después de un pequeño rato es porque hay algún problema en la red, y si sigue con la instalación, al final dará error.

## NIVELES FUNCIONALES

Si se va a tener que utilizar SAMBA para tener un DC adicional estando en linux, al crear el dominio es recomendable ponerlo en 2008, porque SAMBA únicamente funciona en ese nivel funcional. Sinó, pues se deja en 2016.

