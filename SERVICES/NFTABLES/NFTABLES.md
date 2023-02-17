#CVSKILLS 

## WEBGRAFÍA

- MANPAGE: https://www.mankier.com/8/nft#

## INTRODUCCIÓN

Este tipo de servicio es utilizado principalmente para crear reglas de filtrado, nat, etc para el correcto funcionamiento de la red. Permitiendo también tener una mayor seguridad.

Es el predecesor del antiguo **IPTABLES**

## PALABRAS CLAVES
`INPUT`: Gestiona todos los paquetes que tiene como destino la **misma** **máquina**.

`OUTPUT`: Todos los paquetes que tienen como origen la **propia máquina**

`FORWARD`: Todos los paquetes que atraviesan la máquina. Reevía los paquetes entre interfaces/direcciones

`PREROUTING`: Paquetes que entran en la máquina antes de ser enrutados

`POSTROUTING`: Paquetes que están a punto de salir de la máquina

![](Pasted%20image%2020230217175140.png)

