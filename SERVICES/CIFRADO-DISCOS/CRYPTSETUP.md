#linux 

# CRYPTSETUP

Es el programa utilizado para usar diversas funciones de encriptado de discos, etc.

## PROCESO DE INSTALACIÓN

INSTALAR: `apt install cryptsetup`

## FICHEROS DE CONFIGURACIÓN IMPORTANTES

`/etc/crypttab`

## NOTAS VARIAS (IMPORTANTES)



# LUKS

## Manejo de discos cifrados [FUENTE](http://persoal.citius.usc.es/tf.pena/ASR/Tema_3html/node14.html#SECTION00026100000000000000)

El cifrado que se usa en el proceso de instalación es un cifrado de disco completo (Full disk encryption, FDE)

-   Se cifran todos los bits del disco o de la partición
-   Es diferente del cifrado a nivel de sistema de ficheros (Filesystem-level encryption, FLE) en el que se cifra el contenido de los ficheros, no los metadatos (nombre del fichero, fechas de modificación, etc.).

El subsistema de cifrado FDE en Linux 2.6 es [dm-crypt](http://en.wikipedia.org/wiki/Dm-crypt)

-   Parte de la infraestructura device mapper, utilizada también en LVM2 o RAID software, y puede colocarse por encima de estos
    -   Permite cifrar discos completos, particiones, volúmenes lógicos o volúmenes RAI software
-   Comando básico: cryptsetup
-   Permite utilizar el estándar [LUKS](http://code.google.com/p/cryptsetup/) (Linux Unified Key Setup)
-   Fichero /etc/crypttab: indica en el arranque como descifrar los discos

### LUKS

Estándar para cifrado de disco en Linux

-   Facilita la compatibilidad entre distribuciones
-   Incluye soporte para múltiples claves
-   Revocación de contraseña efectiva
-   Uso mediante el comando cryptsetup, con dm-crypt como backend

### cryptsetup

Comando básico para el cifrado de disco

-   Puede cifrar con o sin LUKS
-   LUKS no permite cifrado con clave aleatoria, generada a partir de /dev/random o /dev/urandom[5](http://persoal.citius.usc.es/tf.pena/ASR/Tema_3html/footnode.html#foot1926)

Ejemplos de uso:

1.  Cifrar y activar una partición usando formato LUKS y una contraseña como clave (todos los datos se pierden y debemos reinicir el filesystem)  
    ## Para mayor seguridad, desmonta y sobreescribe  
    ## la partición (puede ser muy lento)  
    umount /dev/hda8  
    dd if=/dev/urandom of=/dev/hda8  
    ## Formatea la partición, cifrando con LUKS  
    cryptsetup luksFormat /dev/hda8  
    ## Activa la partición, en el  
    ## dispositivo /dev/mapper/hda8_crypt  
    cryptsetup luksOpen /dev/hda8 hda8_crypt  
    ## Reinicia el sistema de ficheros  
    mkfs -t fstipo /dev/mapper/hda8_crypt  
    ## Obtiene el UUID luks  
    crypsetup luksUUID /dev/hda8
2.  Cifrar una partición de swap usando /dev/urandom como clave aleatoria (sin usar LUKS)
    
    cryptsetup create hda7_crypt /dev/hda7 --key-file /dev/urandom
    
3.  Usar un fichero de clave para una partición cifrada inicialmente con contraseña, eliminando la contraseña inicial
    
    > # Crea un fichero aleatorio para usar como clave  
    > dd if=/dev/urandom of=/etc/keys/hda6.luks bs=1 count=4096  
    > # Cambia los permisos del fichero (debe ser de root)  
    > chown root.root /etc/keys/hda6.luks  
    > chmod 700 /etc/keys  
    > chmod 400 /etc/keys/hda6.luks  
    > # Añade el fichero hda6.luks como clave  
    > cryptsetup luksAddKey /dev/hda6 /etc/keys/hda6.luks  
    > # Comprueba que existen dos claves (slots)  
    > cryptsetup luksDump /dev/hda6  
    > # Borra el slot 0 con la clave original  
    > # (impide usar esa clave, hacerlo solo después de comprobar que el fichero luks funciona como clave)  
    > cryptsetup luksKillSlot /dev/hda6 0 -key-file /etc/keys/hda6.luks  
    > # Comprueba que el slot se ha borrado  
    > cryptsetup luksDump /dev/mapper/hda6_crypt  
    > # Por último, modifica el fichero crypttab para indicar que se use el fichero luks
    
4.  Extiende el dispositivo cifrado GV-homelv_crypt, después de haber extendido el volumen lógico sobre el que está definido
    
    > # cryptsetup resize /dev/mapper/GV-homelv_crypt
    

### Fichero /etc/crypttab

Especifica en el arranque como se deben descifrar los discos  
Ejemplo:
```
#<cifrado>  <original> <fichero clave>      <opciones>
hda7_crypt  /dev/hda7  /dev/urandom         cipher=aes-cbc-essiv:sha256,size=256,swap
hda8_crypt  /dev/hda8  none                 luks
hda6_crypt  /dev/hda6  /etc/keys/hda6.luks  luks
```
**Línea 1**

área de swap con cifrado aleatorio

**Línea 2**

partición con cifrado LUKS; none indica que se pide una contraseña en el arranque

**Línea 3**

partición con cifrado LUKS y clave obtenida desde fichero

Para evitar problemas (posibles cambios en el nombre de las particiones), es preferible substituir el nombre del dispositivo original por su UUID obtenido usando crypsetup luksUUID

## cryptsetup luksUUID /dev/hda8
0e44dcc3-2b1f-4349-9fb3-db82b547acd1
## cat /etc/crypttab

hda8_crypt UUID=0e44dcc3-2b1f-4349-9fb3-db82b547acd1 none luks



# LUKS/cryptsetup Tutorial for Linux Hard Drive Partition Encryption [FUENTE](https://realtechtalk.com/LUKScryptsetup_Tutorial_for_Linux_Hard_Drive_Partition_Encryption-1002-articles)

This is based on Debian Linux but should apply equally to any *nix distro.

### Install LUKS/crypt-setup

`apt-get install cryptsetup`

### Setup your LUKS Partition

Of course change /dev/md2 with whatever partition you intend to use LUKS on.

`cryptsetup --verbose --verify-passphrase luksFormat /dev/md2`

You'll be asked to verify your decryption password twice

***DO NOT FORGET THIS PASSWORD AS IT IS NOT RECOVERABLE!**

### Open/Unlock your LUKS Partition

`cryptsetup luksOpen /dev/md2 mylukspartition`

`You'll be asked for your passphrase at this point (the one you entered above, hopefully you haven't forgotten it already!)`

You can change "mylukspartition" to whatever you would like to call it, it just controls the name created in /dev/mapper which is the device you will use to mount the encrypted LUKS partition.

**You'll find that the above command creates** **/dev/mapper/mylukspartition**

This will create the device for your LUKS partition (remember you will never be able to open it directly and you'll always need the LUKS tools to unlock the partition, so keep this in mind when using a Live/Recovery CD etc...)

### CREATE the filesystem on the LUKS device

`mkfs.ext3 /dev/mapper/mylukspartition`

***Of course you can use any filesystem over top of LUKS but most will probably use ext3**

### Mount the LUKS Partition

`mkdir /mnt/luks`

`mount /dev/mapper/mylukspartition /mnt/luks`

### To Umount and Secure Your Data

`cryptsetup luksClose /dev/mapper/mylukspartition`

Now your data is safe and in order to mount (luksOpen) you will need the passphrase which only you should know.
