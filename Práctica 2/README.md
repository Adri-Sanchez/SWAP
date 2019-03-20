# Práctica 2  
  
## Copia de archivos por SSH
Para realizar una copia de los archivos entre dos máquinas, se puede utilizar la orden **ssh** creando un tar.gz en el equipo destino 
sin necesidad de crearlo previamente en el sistema principal. Para ello, introduciremos el siguiente comando:  
*tar czf - directorio | ssh IPDestino 'cat > ~/tar.tgz'*. Podemos ver un ejemplo de uso en la imagen a continuación:

![tar](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%202/Capturas/tar.PNG)

Creamos un directorio llamado *Test* en la máquina origen. Copiamos el archivo mediante SSH y observamos que se crea un fichero *tar.tgz* en
la segunda máquina.  
  
  
## Clonado mediante Rsync  
Para usar la herramienta Rsync primeramente hemos de descargarla e instalarla en nuestro sistema (si no lo está actualmente) mediante el uso de apt-get.  
  
Haremos uso de Rsync como usuario sin privilegios, por lo tanto, es recomendable que dicho usuario sea propietario de la carpeta donde 
nos gustaría trabajar. Por ello, haremos uso de: *sudo chown adrisc:adrisc -R /var/www*, en este caso. 
  
A continuación, vamos a comprobar los archivos existentes actualmente en la máquina origen, en el directorio */var/www/*

![rsync1](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%202/Capturas/rsync1.PNG)
  
Para clonar los contenidos de este directorio a nuestro servidor secundario, haremos uso de la siguiente orden:  
*rsync -avz -e ssh IPOrigen:/var/www/ /var/www/*. Una vez hemos introducido dicho comando, nos pedirá ingresar la contraseña de la máquina origen. 
En caso de ser correcta, comenzará con el clonado de directorios y archivos en dicha localización.

![rsync2](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%202/Capturas/rsync2.PNG)
  
  
## Configuración SSH sin contraseña  
Si deseamos utilizar el protocolo SSH evitando introducir la contraseña cada vez que queremos ingresar en una máquina cualquiera, debemos copiar nuestra
clave pública al equipo remoto.   
  
Primeramente, debemos generar un par de claves privada-pública mediante el uso de **ssh-keygen**. En nuestro caso, haremos uso de esta orden 
de la siguiente manera: *ssh-keygen -b 4096 -t rsa*. En el momento de la generación, nos pedirá una contraseña para añadir
seguridad extra a las claves; dado que no queremos introducir más la contraseña cuando realicemos accesos mediante por SSH, debemos dejarla en blanco.

![ssh1](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%202/Capturas/ssh1.PNG)
  
Por último, para copiar la clave generada a la máquina destino de forma muy sencilla, podemos hacer uso de la orden *ssh-copy-id* 
indicando la dirección IP remota de la máquina en la que queremos copiar nuestra clave pública.   
  
A continuación se muestra cómo copiar la clave pública y el posterior ingreso a la máquina remota, mediante SSH, sin necesidad de 
introducir la contraseña.

![ssh2](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%202/Capturas/ssh2.PNG)

## Configuración crontab  
Si queremos realizar copias de seguridad y sincronizaciones de forma periódica haremos uso de *cron*, un administrador de procesos en segundo plano que nos permite programar 
tareas de forma muy sencilla.
  
Para programar una tarea con cron, basta con editar el archivo **/etc/crontab**. En nuestro caso, queremos que rsync realice una 
sincronización, cada minuto, del directorio */var/www/* desde la máquina principal hacia la máquina secundaria. 
  
La estructura del fichero sería la siguiente:  
![cron1](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%202/Capturas/cron1.PNG)

Para probar el uso y correcto funcionamiento de dicha sincronización, veremos como cambia un archivo cualquiera en la máquina secundaria, tras modificar su contenido en
la máquina principal, con el transcurso de un minuto.

![cron2](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%202/Capturas/cron2.PNG)


