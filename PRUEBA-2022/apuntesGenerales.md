# APUNTES GENERALES

## NETWORK

### CAMBIAR NOMBRE DE ADAPTADOR DE RED

#### PERSISTENTE ==COMPLETAR...==

Para hacer persistente el cambio (es lo que interesa) tendremos que modificar el fichero 

`/etc/udev/rules.d/70-persistent-net.rules`

```
# Interfaz con MAC "xx:xx:xx:xx:xx:xx" asignado a enp0s3

SUBSYSTEM=="net",
```

 ==**NO PERSISTENTE**==

Apagamos la interfaz:

`ip link set <nombreInterfaz> down`

Cambiamos el nombre y la volvemos a iniciar para que los cambios se apliquen a la misma vez que la interfaz se inicie.

`ip link set <nombreInterfaz> name <nuevoNombreInterfaz> up`

### OPCIONES FICHERO `/etc/network/interfaces`

```
#Permitir conexión en caliente
allow-hotplug <nombreInterfaz>

#Configuración de las direcciones en DHCP
iface enp0s3 inet dhcp

#Configuración de las direcciones en STATIC
iface enp0s3 inet static
	address 192.168.1.1/24
	gateway 10.0.2.2
#Añade servidores dns manualmente
	dns-nameservers 8.8.8.8 
```

#### CONFIGURACIONES DNS

En el fichero `/etc/resolv.conf` se pueden indicar manualmente los dominios de DNS.