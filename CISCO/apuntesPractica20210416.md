# PRÁCTICA 2021 04 16

## COMANDOS

### CONFIGURACIÓN DE INTERFACES

```
R1(config)# interface gig6/0 // Configurar la interfaz gig6/0
R1(config-if)# no shutdown // Encender la interfaz

```

### CONFIGURACIÓN DEL ROUTER 

```
R1> enable // Modo Exec en dispositivo de cisco
R1(config)# hostname R1 //Renombrar el router

```

### CONFIGURACIÓN VLANs (SWITCHES)

#### ACCESS

```
S1(config)# interface NombreInterfaz // Seleccionamos la interfaz
S1(config-if)# switchport mode access // Activamos el modo de vlan acceso 
S1(config-if)# switchport access vlan EtiquetaVLAN // VLAN a la que pertenece

```

#### TRUNK

```
S1(config)# interface NombreInterfaz // Seleccionamos la interfaz
S1(config-if)# switchport mode trunk // Activamos el modo de vlan troncal 
S1(config-if)# switchport trunk allowed vlan 10 // Activar la VLAN 10

/* OPCIONAL */
S1(config-if)# switchport trunk native vlan 10 // Nativa la VLAN 10 en el trunk 
```

#### CONFIGURACIONES GENERALES

```
S1(config)# vlan 10 // Indicamos la VLAN a configurar
S1(config-vlan)# name NombreVlan // Cambiar el nombre de la VLAN
```

MÁS INFO [VLANs NATIVAS](VLAN.md#TRUNK)

### INFORMACIÓN VLANs

```
S1# show vlan brief // Mostrar un resumen de todas las VLANs
```

## ROUTER

### SUBINTERFACES EN EL ROUTER

Esta opción se utiliza en los modelos de red que se llaman ROUTER ON A STICK.

Actuará como GATEWAY para los equipos de la VLAN especificada

>- **Subinterfaces**: Para elegir la subinterfaz, lo que hay que hacer es primero saber cual es la interfaz a utilizar, luego cuando ya lo sabemos, para indicar la subinterfaz ponemos el puerto en el que está y le ponemos `6/0.10` (*por ejemplo*: la interfaz **6/0**)
>- Cada subinterfaz para cada VLAN
>- Todas las subinterfaces comparten la misma MAC (por lo menos en dispositivos CISCO)
>- No se puede apagar una subinterfaz, únicamente a interfaces "padres"
>- Las subinterfaces se **enrutan** entre sí, porque están en la misma interfaz. Para evitar que se enruten, habría que hacer uso de las ACLs.
>- Habrá que configurarle una **ip por cada subinterfaz**

```
R1# config terminal
R1(config)# interface gigabitethernet 6/0.10 // Elegir la subinterfaz
R1(config-subif)# encapsulation dot1Q 10 // Encapsulamos la subif con la VLAN 10
```



### COMANDOS EXTRA

```
[ctrl + 6]: Cuando te equivocas en una palabra y empieza a buscar servidores de dominio, este comando deja de buscar.

R1(config)# no ip domain-lookup // Directamente deshabilita las búsqueda de servidores de dominio
```

## NOTAS

>- Es importante apagar las interfaces que no se están utilizando
>- No hace falta crear las VLANS ya que cunado se asignan a un puerto se crean automáticamente