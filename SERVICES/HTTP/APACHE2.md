
## CONFIGURAR TLS1.3 EN APACHE

https://apache.tutorials24x7.com/blog/how-to-enable-tls-1-2-and-tls-1-3-in-apache-web-server

## EXPLICACIÓN DE LA IA

To configure Apache2 on a Linux server, you will need to follow these general steps:

1.  Install Apache - use the package manager of the Linux distribution to install the Apache2 package. For example, on Ubuntu/Debian, you can use the command `sudo apt-get install apache2`.
    
2.  Configure the server - The configuration for Apache is located in the `/etc/apache2` directory. Common files to configure include `apache2.conf` and `sites-available/default`. You can view and edit these files using a text editor like `nano` or `vim`.
    
3.  Enable Modules - Apache has various modules that provide additional functionality. To enable a module, you can use the `a2enmod` command. For example, to enable the rewrite module, you can use `sudo a2enmod rewrite`.
    
4.  Create Virtual Hosts - Virtual Hosts allow you to host multiple websites/domains on a single server. To create a virtual host, create a new configuration file in the `/etc/apache2/sites-available/` directory. Make sure the file ends with `.conf`. Then, use the command `sudo a2ensite <sitename>.conf` to enable the site.
    
5.  Restart Apache - After you've made changes to the configuration files for Apache, you'll need to restart the Apache service using the command `sudo systemctl restart apache2`.
    

Note that this is a general overview, and the specific steps and configuration options may vary depending on your Linux distribution and the specific requirements of your project.