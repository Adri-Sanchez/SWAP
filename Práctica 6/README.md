# Práctica 6

## Descripción inicial  
En esta práctica el objetivo será configurar un servidor NFS para exportar un espacio en disco
a los servidores finales, los cuales actuarán como clientes NFS.  
  
## Configurar el servidor NFS  
En la nuestra máquina servidor, instalaremos las siguientes herramientas para poder llevar a cabo
la configuración:

```bash
sudo apt-get install nfs-kernel-server nfs-common rpcbind
```  

Una vez ha finalizado la instalación, creamos una carpeta que actuará de carpeta compartida entre los 
distintos equipos que conforman la red configurada bajo NFS.

```bash
mkdir /dat/compartida
```  

A continuación cambiamos el propietario y los permisos de dicha carpeta:

```bash
sudo chown nobody:nogroup /dat/compartida/
sudo chmod -R 777 /dat/compartida/
```  

Para dar permiso de acceso en la carpeta a las máquinas cliente, debemos modificar el archivo de configuración 
*/etc/exports*, indicando las IP de las máquinas cliente:

![exp](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%206/Capturas/exp.PNG)  

Finalmente reiniciamos el servicio para aplicar los cambios.

```bash
sudo service nfs-kernel-server restart
```

## Configurar los clientes  
En las máquinas cliente, necesitamos instalar los siguientes paquetes:

```bash
sudo apt-get install nfs-common rpcbind
```  

Una vez ha finalizado la instalación, crearemos el punto de montaje para la carpeta compartida por cada uno 
de los clientes a configurar.

```bash
cd /home/user
mkdir carpetacliente
chmod -R 777 carpetacliente
```

Una vez creada la carpeta, solo queda montar la carpeta remota exportada desde el servidor sobre el directorio creado.

```bash
sudo mount {IP del servidor}:/dat/compartida carpetacliente
```  

En este punto podemos comprobar que se puede leer y escribir sobre los archivos almacenados en la carpeta y 
que los cambios se actualizan en todas las máquinas, resto de clientes inclusive.  

Para hacer la configuración permanente, modificaremos el archivo de configuración */etc/fstab* para que la carpeta se monte 
al arrancar el sistema:

![fstab](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%206/Capturas/fstab.PNG)