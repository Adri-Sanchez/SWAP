# Práctica 4

## Descripción inicial
En esta práctica, aseguraremos nuestra granja web mediante la instalación de un certificado SSL y configurando reglas para el cortafuegos, de tal forma que solo se permitirá el tráfico que consideremos oportuno como administradores.

## Instalar un certificado SSL autofirmado
Con un certificado SSL, podemos proporcionar seguridad al usuario que haga uso del servicio ofrecido. SSL o *Secure Sockets Layer* es un protocolo de comunicación que se ubica en la pila de protocolos sobre TCP/IP, proporcionando servicios de  comunicacion segura entre cliente y servidor.  

Para generar un certificado autofirmado de este tipo, debemos activar el módulo SSL de Apache en nuestra granja web, para ello, ejecutaremos los siguientes comandos:

```bash
a2enmod ssl
service apache2 restart
```

Una vez reiniciado el servicio de Apache, procedemos creando una carpeta para albergar los certificados generados.

```bash
mkdir /etc/apache2/ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt
```
Nos pedirá diversos datos, como por ejemplo la localización y el nombre de organización para configurar el dominio.  
Una vez finalizado el proceso, editamos el archivo de configuración */etc/apache2/sites-available/default-ssl.conf* agregando lo siguiente:  

![sslconf](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%204/Capturas/sslconf.PNG)

Activamos el sitio default-ssl y reniciamos Apache:

```bash
a2ensite default-ssl
service apache2 reload
```

En este punto se pueden realizar peticiones por medio de HTTPS usando CURL.   
![https](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%204/Capturas/https.PNG)

Para que la granja web haga uso de HTTPS, podemos copiar los certificados generados anteriormente a las otras máquinas mediante, por ejemplo, Rsync. Además, configuraremos el balanceador para que acepte este tráfico (puerto 443). Editaremos el fichero de configuración localizado en */etc/nginx/conf.d/default.conf* del balanceador.

![lbconf](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%204/Capturas/lbconf.PNG)

Ya podemos hacer peticiones por medio de HTTPS al balanceador.  

![testlb](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%204/Capturas/testlb.PNG)

## Configuración de cortafuegos

Para configurar el cortafuegos, usaremos IPTABLES. Buscamos que en el inicio de nuestro sistema, el cortafuegos se encuentre correctamente configurado. Por ello, una opción es crear un script que se ejecute al inicio del sistema.

![iptables](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%204/Capturas/iptables.PNG)

Para que nuestro script se ejecute cada vez que encendemos nuestra máquina, bastará con incluir el comando de su ejecución dentro de */etc/rc.local*

![iptables2](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%204/Capturas/iptables2.PNG)


  
