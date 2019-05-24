# Práctica 5

## Descripción inicial  
En esta práctica, el objetivo será el de realizar copias de seguridad de nuestras bases de datos
MySQL usando una réplica maestro-esclavo, de manera que un servidor en producción hace de maestro,
y otro servidor, que actúa como copia de seguridad, hace de esclavo.  
  
## Creación de la base de datos  

Para llevar a cabo la creación de una base de datos, haremos uso del comando:

```bash
mysql -u root -p
```  

Una vez hemos iniciado en el monitor de MySQL, creamos una nueva base de datos, dotándola de un
nombre a nuestra preferencia. Posteriormente usaremos dicha base de datos para insertar datos por lo que, inicialmente:

```bash
mysql> create database contactos;
mysql> use contactos;
```  

Una vez hemos creado la base de datos, procedemos a crear un registro de datos como sigue 
en la siguiente imagen:  

![sql1](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%205/Capturas/sql1.PNG)  

## Copia de seguridad  
Para llevar a cabo la copia de seguridad, haremos uso de la herramienta mysqldump. De forma inicial, 
debemos ingresar en el monitor de MySQL y ejecutar lo siguiente en el servidor que alberga la base de datos principal:  
  
```bash
mysql> FLUSH TABLES WITH READ LOCK;
mysql> quit
```  

Una vez realizado el paso anterior, procedemos con mysqldump para guardar los datos en el servidor principal.  

```bash
mysqldump contactos -u root -p > /tmp/contactos.sql
```  

Si se ha generado el archivo correctamente, procedemos a desbloquear las tablas en MySQL.

```bash
mysql -u root -p

mysql> UNLOCK TABLES;
mysql> quit
```

Ahora podemos ir a la máquina esclavo o máquina secundaria para copiar el archivo .SQL generado que contiene
los datos guardados de la base de datos inicial.

![scp](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%205/Capturas/scp.PNG)  

Una vez disponemos del archivo de copia de seguridad en el esclavo, podemos importar la base de datos a MySQL.
En un primer paso, creamos la base de datos con el mismo nombre y restauramos los datos usando el siguiente comando:

```bash
mysql -u root -p contactos < /tmp/contactos.sql
```  

## Configuración maestro-esclavo  
La opción anterior puede resultar un poco lenta con grandes bases de datos y requieren de la intervención de un operador para realizarla. 
MySQL posibilita la configuración de un proceso demonio para hacer la replicación de las bases de datos en la estructura maestro-esclavo de forma sencilla.  
Primeramente accederemos al archivo de configuración */etc/mysql/mysql.conf.d/mysqld.cnf* y comentaremos el parámetro **bind_address** que sirve
para que se establezca una escucha a un servidor. Posteriormente, establecemos el identificador del servidor **server-id = 1** y finalmente estableceremos la ruta para 
almacenar los log de errores y registro de actualizaciones: *log_error = /var/log/mysql/error.log* y *log_bin = /var/log/mysql/bin.log*. Guardamos el 
documento y reiniciamos el servicio mediante:

```bash
/etc/init.d/mysql restart
```  

Si no ha avisado de ningún tipo de error, hemos finalizado la configuración del maestro. Procedemos a configurar el esclavo 
la cual será muy similar a la realizada anteriormente excepto a la hora de establecer el identificador del servidor, que será **server-id = 2**.  

Tras completar con éxito ambas configuraciones, podemos volver al servidor maestro para crear un usuario y proporcionarle permisos de replicación:

![me22](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%205/Capturas/me22.PNG)   

Para finalizar con la configuración en el maestro, obtenemos los datos de la base de datos a replicar, tal y como se puede ver en la imagen anterior, mediante:

```bash
mysql> SHOW MASTER STATUS;
```  

Volvemos a la máquina esclavo y configuramos los distintos parámetros dependiendo de la salida que hemos recibido anteriormente.

![me3](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%205/Capturas/me3.PNG)  

Por último, arrancamos el servicio esclavo que replica de forma automática la información mediante:

```bash
mysql> START SLAVE;
```  

Y desbloqueamos las tablas en el maestro:
```bash
mysql> UNLOCK TABLES;
```

Hemos finalizado la configuración, solo queda probar su funcionamiento y comprobar si, de forma correcta, se replican los datos introducidos en la máquina maestro.

![sql2](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%205/Capturas/sql2.PNG)