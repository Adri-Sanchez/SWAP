# Práctica 1

## Instalación del sistema operativo y herramientas necesarias
Para la realización de las prácticas se va a utilizar el entorno de virtualización VirtualBox.  
Inicialmente, debemos crear una máquina virtual e instalar el sistema operativo adecuado (Ubuntu Server 16.04 en nuestro caso), 
asegurándonos de incluir la instalación completa de un servidor web indicando la instalación de los servicios SSH y LAMP.  

Debido a que para completar la práctica es necesario el uso de dos máquinas virtuales, se debe añadir una interfaz de red adicional
configurada como una 'Red Interna', por lo tanto, en cada máquina dispondremos de dos interfaces: 
 
* **NAT** - Permite el acceso a internet
* **Red Interna** - Permite que las máquinas sean 'visibles' entre sí.

![img1](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%201/Capturas/red1.PNG)
  
## Interfaces de red
Una vez hemos establecido las interfaces en la configuración de VirtualBox, procedemos a iniciar la máquina virtual y, mediante el uso
de un editor de texto cualquiera, actualizaremos el archivo localizado en */etc/network/interfaces/* como procede en la siguiente imagen, 
de forma que incluyamos la información relativa a la segunda interfaz que hemos establecido y que permite que las máquinas virtuales puedan 
establecer conexión entre sí. 

![img2](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%201/Capturas/red2.PNG)

En este punto, una vez hemos establecido la dirección IP estática para dicha máquina, podemos clonar la máquina virtual y 
crear otra nueva. Debemos modificar la dirección IP o *address* para diferenciarla de la primera máquina.

Reiniciamos ambas máquinas y visualizamos las interfaces de red mediante el uso del comando **ifconfig**

**Interfaces de la máquina 1**
  
![img3](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%201/Capturas/red3.PNG)

**Interfaces de la máquina 2**
  
![img4](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%201/Capturas/red4.PNG)

  
Para comprobar que ambas máquinas pueden visualizarse en red entre sí, hacemos un ping desde la máquina 2 hacia la máquina 1
  
![img5](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%201/Capturas/red5.PNG)
  
Las interfaces creadas funcionan correctamente tal y como se puede observar.  
Comprobamos ahora si las máquinas virtuales disponen de conexión a internet, para ello haremos ping a una IP pública accesible 
como podría ser la de Google (8.8.8.8)

![img6](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%201/Capturas/red6.PNG)

Parece ser que no disponemos de conexión a internet a pesar de haber reiniciado las máquinas tras establecer las interfaces, 
por lo tanto, vamos a probar reinicializando el servicio de red mediante el uso de la orden **systemctl restart networking.service**.

![fix1](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%201/Capturas/redfix1.PNG)
  
El inconveniente con esta medida es que cada vez que reiniciamos la máquina es necesario ejecutar el comando anteriormente mencionado.
Una posible solución sería la de ejecutar dicho comando con la carga del sistema operativo, para ello, modificamos
el archivo *etc/rc.local*, utilizado para ejecutar comandos o scripts cada vez que iniciamos el sistema, mediante cualquier editor.  

![fix2](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%201/Capturas/redfix2.PNG)
  
  
## Conexión mediante CURL

CURL es utilizado para transferir archivos con sintaxis URL. Para mostrar el funcionamiento de esta herramienta, se debe crear un
fichero HTML en una de las máquinas, localizado en */var/www/html/*. En este caso el fichero (hola.html) será el siguiente:

![curl1](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%201/Capturas/curl1.PNG)

Para usar CURL, utilizaremos la orden **curl http://DestIP/hola.html** desde la otra máquina virtual.

![curl2](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%201/Capturas/curl2.PNG)
  
  
## Conexión mediante SSH

Para establecer una conexión mediante SSH (Secure Shell) desde una máquina hacia la otra, debemos usar el protocolo con el comando
**ssh Username@IP**. Para este caso no sería necesario escribir el usuario.  
Una vez nos hemos conectado a la otra máquina de forma remota, creamos un directorio para comprobar el correcto funcionamiento de SSH.  

![ssh1](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%201/Capturas/ssh1.PNG)
![ssh2](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%201/Capturas/ssh2.PNG)

Como se puede observar, nos hemos conectado y creado el directorio de forma remota con el uso de SSH.
