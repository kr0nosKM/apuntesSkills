# PUBSRV

CONEXIÓN CON: 

INFORMACIÓN ADICIONAL:
- [Nombre adaptador](apuntesGenerales#network)

## ANTECEDENTES

pubsrv Instala y/o configura: 
1. 
	1. Un Certificado de Autoridad Raíz con los siguientes campos: C=ES O=WSC CN=WSC Root CA Ubica el certificado (ca.crt) en el directorio /etc/ssl/certs. 
	2. Expide y firma los certificados necesarios, que se ubicarán en los directorios correspondientes (certs y private) de /etc/ssl, y contendrán los siguientes campos: C=ES O=WSC CN= o Asegúrate de que todos los servidores y clientes aceptan certificados expedidos por esta WSC Root CA.
	3. En todas las máquinas, los certificados se ubicarán en los directorios correspondientes (certs y private) de /etc/ssl.
2. Servidor DNS (bind9) para la zona directa umh.es. Las peticiones DNS de spainskills.es deberán ser reenviadas a fwsede. 
3. Servidor web (apache2) de www.umh.es, que muestra el mensaje “Web oficial de UMH”, accesible por HTTP y HTTPS. Utiliza un certificado expedido por WSC Root CA (www.umh.es). 
4. Servidor de correo electrónico (postfix y dovecot) accesible en mail.umh.es, para enviar y recibir emails del dominio umh.es.
	1. Los usuarios locales (ver anexo V) acceden a sus buzones de entrada (ver anexo V) usando el protocolo IMAP securizado con STARTTLS (puerto 143) y envían emails usando el protocolo SMTP securizado con STARTTLS (puerto 587). Utiliza un certificado expedido por WSC Root CA (mail.umh.es).

---

## PROCESO

### CONFIGURACIÓN  DE RED

[Modificar el **nombre del adaptador** y **opciones del fichero** ...../interfaces](apuntesGenerales#network)

Modificar el fichero :`/etc/network/interfaces`

```
...
allow-hotplug ens18
iface ens18 inet static
	addresss 20.22.0.39/24
	dns-nameservers 127.0.0.1
...
```

### CONFIGURACIÓN CA

>**POR SI ACASO NO VIENE INSTALADO**
>`apt install openssl`

1. **CREACIÓN CA PRIVATE KEY**
	1. `openssl genrsa -out /etc/ssl/private/rootCA.key 4096`
	2. EXPLICACIÓN:
		1. `genrsa`: Indicamos que queremos utilizar una función de openssl para crear claves RSA
2. **CREACIÓN DEL CERTIFICADO AUTOFIRMADO DE LA CA ROOT**
	1. COMANDO UTILIZADO EN LA PRÁCTICA
		1. `openssl req -x509 -new -nodes -key /etc/ssl/private/rootCA.key -subj "/C=ES/O=WSC/CN=WSC Root CA" -sha256 -days 365 -out /etc/ssl/certs/rootCA.crt`
	2. <u>EXPLICACIÓN</u>:
		1. `req`: Indicamos que queremos utilizar una función de openssl que gestiona las peticiones de certificados (requests) y también tiene una utilidad de generar certificados.
		2. `-x509`: Queremos utilizar el estándar X.509 el cual especifica una serie de terminos (pej. algoritmos, sintaxis, etc.)
		3. `-new`: 
		4. `-nodes`
		5. `-key <clavePrivada>.key`
		6. `[-subj "/Atributo0=Valor0/Atributo1=Valor1...."]` No poner si se utiliza el modo interactivo. Sus opciones pueden ser muchas, pero en este caso usaré estas:
			1. C (country): Indicar el pais
			2. O (organization): Indicar la organización a la que pertenece
			3. CN (common name) **IMPORTANTE**: Indicar el nombre de la CA 
		7. `-sha256`: Método de encriptación
		8. `-days <numeroDias>`
		9. `-out <certificadoDeSalida>`
>**MODO INTERACTIVO:**
 El modo interactivo es no poner los atributos directamente en el comando, sinó que el propio openssl lo va preguntando poco a poco.