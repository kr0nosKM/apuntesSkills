#linux 

# SAMBA

## INTRODUCCIÓN

Es un protocolo de comunicaciones para poder compartir carpetas entre windows y linux y que no hayan incompatibilidades. Existen otros tipos de protocolos como `NFS`.

El fichero de configuración más importante es `/etc/samba/smb.conf`, es donde se realizan la mayoría de las configuraciones.

Existen 3 servicios (demonios) principales al instalar samba:
- **smbd**
	- Proporciona servicios de impresión y ficheros a los clientes windows y linux
	- Fichero de configuración: `smb.conf`
- **nmbd**
	- Proporciona soporte de búsqueda en NetBIOS.
	- Fichero de configuración: `smb.conf`
- **winbindd**
	- Este servicio es usado para integrar autenticación y la base de datos del usuario a linux

## COSAS A TENER EN CUENTA

- **Comprobar la sintaxis**
	- Importante comprobar la sintaxis antes de hacer cualquier cosa con el fichero de configuración, para ello lo que habrá que hacer es utilizar el comando `testparm /etc/samba/smb.conf`
- **Secciones reservadas**
	- [global]
		- Se aplican al servidor completo. Son como parámetros por defecto para los directorios.
	- [homes]
		- Los servicios que conectan clientes en sus directorios HOME son creados por el server.
	- [printers]
		- Funciona igual que homes pero con impresoras.
- **Regla Firewall**
	- Importante agregar una regla en el firewall para que todo funcione correctamente (en el caso de que el firewall lo tengamos como restrictivo)

## CONFIGURACIÓN

> apt install samba

```
[NOMBRE-CARPETA-COMPARTIDA]
	comment = Descripción de la carpeta compartida
	path = /ruta/carpeta (ruta de la carpeta compartida)
	browsable = yes / no (Si se mostrará al inspeccionar la red)
```


## WEBGRAFÍA
- [RedHat Configuration](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/deploying_different_types_of_servers/assembly_using-samba-as-a-server_deploying-different-types-of-servers)
- [Pequeña documentación](https://cambiatealinux.com/instalar-y-configurar-samba-en-ubuntu-linux)
- [Man Page](https://linux.die.net/man/7/samba)
- [Man Page smb.conf](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html)
- [WIKI](https://wiki.samba.org/index.php/Setting_up_Samba_as_a_Standalone_Server)
- [Información General](https://blog.programster.org/samba-cheatsheet)

## INFORMACIÓN DE [MAKEUSEOF](https://www.makeuseof.com/set-up-network-shared-folder-ubuntu-with-samba/)

Bastante importante, en esta página se muestra con bastante detalle alguna de las opciones.

## Step 2: Configuring Samba

To be able to share files securely with other network devices, you have to configure the Samba server. The main configuration file for Samba is located at **/etc/samba/smb.conf** on your PC. This guide uses the Vim text editor for editing the Samba configuration file, but feel free to use any other text editor of your choice.

**Note:** You need to have administrative privileges to edit the configuration file.

 `sudo vim /etc/samba/smb.conf` 

Add the following lines to the bottom of the config file.

 ```
 [sambashare]
 comment= Network Shared Folder by Samba Server on Ubuntu   
 path = /home/your_username/sambashare   
 force user = smbuser   
 force group = smbgroup   
 create mask = 0664   
 force create mode = 0664   
 directory mask = 0775   
 force directory mode = 0775   
 public = yes   
 read only = no   
 ``` 

Remember to update the **path** parameter with your username. You can get your username by running the following command:

 `echo $USER` 

To [exit the Vim editor](https://www.makeuseof.com/vim-save-file/) after making your changes, simply type **:wq** and press the **Enter** key.

### Understanding the Configurations

Here is a brief description of the configuration lines that you just added.

-   **Section**: A new section in the configuration file is represented by square brackets (**[ ]**). In this case, the section is **[sambashare]**.
-   **Comment**: This line of code provides a brief outline of what this section is about. Especially, it is useful if you have several shared directory sections in the config file.
-   **Path**: This is the path to the directory of your designated network shared folder.
-   **Force user**: The system user that the Samba server will use for sharing files.
-   **Force group**: The name of the group to which the Samba system user will belong.
-   **Create mask**: This parameter will set permissions for newly created files in the shared folder. In this case, the value is 0664 which means that the owner of the file and the group will have read and write permissions while other users will only have read permissions.
-   **Force create mode**: Works in conjunction with the **create mask** parameter in order to set the correct file permissions.
-   **Directory mask**: This parameter determines the permissions for folders in the shared folder. Permissions of 0775, means that the owner and the group have read, write, and execute permissions, while others have read and execute permissions only.
-   **Force directory mode**: This parameter works in collaboration with the **directory mask** to make sure that the correct directory permission is set.
-   **Public**: This parameter specifies that this is a public folder on your network and that other devices can access it.
-   **Read only**: Specifies the permissions for modifying the files within the shared folder.

## Step 3: Creating Samba Resources

Having configured the Samba server, now you have to create the necessary resources such as the Samba user and the directory to share. These resources will facilitate the process of sharing a folder on the network.

### 1. Shared Folder

You need to create the shared folder in the path specified in the Samba config file above. This guide uses a shared folder named **sambashare** located in your home directory.

Navigate to your home directory using [the cd command](https://www.makeuseof.com/cd-command-in-linux/).

 `cd ~` 

Then create the shared directory using the command below:

 `mkdir -p sambashare` 

### 2. Samba User and Group

The next step is to create the Samba system user and group specified in the configuration file.

You can create the Samba system group using the following command:

 `sudo groupadd --system smbgroup` 

Next, create the Samba system user using **useradd**.

 `sudo useradd --system --no-create-home --group smbgroup -s /bin/false smbuser ` 

The command above creates a system user and adds the user to the Samba group created above. Also since this is a system user, no home directory will be created.

### 3. Changing the Shared Folder Owner

Once the Samba user and group are in place, you can now change the shared folder owner to the new user **smbuser** and the group to **smbgroup**. You can achieve this using the command below:

 `sudo chown -R smbuser:smbgroup ~/sambashare` 

Finally, issue the command below to give the group write access to the shared folder and the content inside it.

 `sudo chmod -R g+w ~/sambashare` 

## Step 4: Restarting the Samba Service

You should restart the Samba service for the changes in the Samba configuration file to take effect.

 `sudo systemctl restart smbd` 

After the service restarts, you can check its status with the command below:

 `sudo systemctl status smbd`