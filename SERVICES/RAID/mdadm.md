#linux 

# MDADM

_Multiple Disk and Device Management_

## INTRODUCCIÓN 

Mdadm es un programa de linux orientado a crear RAIDs de diferentes tipos.

## CREACIÓN DE ARRAYS (RAIDs)

### EXPLICACIONES PREVIAS

`/dev/md/nombre`: Cuando indico este dispositivo me refiero a cualquiera dispositivo RAID que tengamos en el directorio `/dev`, puede llegar a tener diferentes nombres.

### RAID 0

`mdadm --create /dev/md/nombre --level=0 --raid-devices=2 /dev/sd[a-b]`

## COMANDOS ESENCIALES

### DETALLES DE LOS RAIDs

`mdadm --detail /dev/md/nombre`


[CHEAT SHEET](https://www.golinuxcloud.com/mdadm-command-in-linux/)
