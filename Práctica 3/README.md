# Práctica 3

## Preparación inicial
En esta práctica, el objetivo es configurar una red entre varias máquinas de forma que una de ellas actúe de balanceador para el resto.

En la máquina dónde vamos a realizar el balanceo por software, debemos ejecutar primeramente las siguientes órdenes:

```bash
sudo apt-get update && sudo apt-get dist-upgrade && sudo apt-get autoremove
```

Una vez realizado el paso anterior, y dado que para el uso de los balanceadores necesitaremos recibir las peticiones HTTP mediante el puerto 80, debemos deshabilitar cualquier servicio utilizando dicho puerto para evitar conflictos. En este caso, Apache está instalado en la máquina y utiliza el puerto 80 por lo cual debemos desactivarlo.

```bash
service apache2 stop
```


## Instalar y configurar NGINX
La herramienta Nginx es un servidor web ligero de alto rendimiento, que podemos instalar de forma sencilla mediante apt:

```bash
sudo apt-get install nginx  
sudo systemctl start nginx
```

Una vez instalado, procedemos a su configuración como balanceador de carga, para ello, necesitaremos modificar dos ficheros:  
  
*/etc/nginx/conf.d/default.conf  
 /etc/nginx/nginx.conf*

Primeramente procedemos a configurar NGinx como balanceador de carga mediante el uso del algoritmo por turnos round-robin.

![nginx_config](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/nginx_config.PNG)
  
Para asegurar el correcto funcionamiento, debemos eliminar la línea que configura a Nginx como servidor web, en el fichero *nginx.conf* mencionado anteriormente.

![nginx_config2](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/nginx_config2.PNG)

Ahora procedemos a reiniciar el servicio.  

```bash
sydo systemctl restart nginx
```

El contenido de las páginas web que vamos a solicitar con el uso de CURL deben ser distintos si queremos comprobar que el funcionamiento es correcto.  
Una vez realizados los pasos anteriores, procedemos lanzando peticiones al balanceador desde una cuarta máquina.

![curl1](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/curl1.PNG)

Tal y como se puede observar, la carga se distribuye correctamente.
  
En el siguiente paso cambiaremos la configuración y estableceremos que el algoritmo utilizado sea mediante ponderación, así, supondremos que la máquina 2 es más potente que la máquina 1. 

![nginx_config3](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/nginx_config3.PNG)

Comprobamos el funcionamiento haciendo peticiones al balanceador:

![curl2](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/curl2.PNG)
  
  
## Instalar y configurar HAProxy
HAProxy es un balanceador de carga y también proxy, de forma que puede balancear cualquier tipo de tráfico. Su instalación se puede realizar fácilmente a partir de apt.

```bash
sudo apt-get install haproxy
```

Una vez instalado, procedemos a detener la ejecución de cualquier servicio que esté utilizando el puerto 80. 

Para llevar a cabo su configuración, modificamos el archivo */etc/haproxy/haproxy.cfg* como sigue:

![haproxy_config](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/haproxy_config.PNG)

Con esta configuración llevaríamos el balanceador aplicaría round-robin para repartir la carga. En caso de querer añadir ponderación como en la instalación anterior, debemos establecer el fichero de configuración de la siguiente forma:

![haproxy_config2](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/haproxy_config2.PNG)

Una vez guardamos la configuración según nuestras preferencias, procedemos a lanzar el servicio de HAProxy mediante
```bash
sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg
```

## Instalar y configurar Pound
Pound es un proxy reverso de configuración simple. Para llevar a cabo su instalación procedemos como sigue:

```bash
sudo apt-get install pound
```

Para configurar Pount debemos modificar el fichero localizado en */etc/pound/pound.cfg*:

![pound_config](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/pound_config.PNG)

Si queremos configurar por ponderación:

![pound_config2](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/pound_config2.PNG)

Para poder iniciar el servicio de Pound necesitamos configurar el archivo localizado en */etc/pound/pound.cfg*:

![pound_config3](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/pound_config3.PNG)


## Realización de pruebas mediante Apache Benchmark

Para realizar el benchmark, se ha creado un fichero HTML conteniendo un script PHP para que la carga de los sistemas sean un poco mayores. 

![bench_html](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/bench_html.PNG)

El comando usado desde la máquina cliente ha sido el siguiente:
```bash
ab -n 10000 -c 100 http://192.168.1.102/bench.html
```

### Nginx
![bench_nginx](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/bench_nginx.PNG)
### HAProxy
![bench_html](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/bench_haproxy.PNG)
### Pound
![bench_html](https://github.com/Adri-Sanchez/SWAP/blob/master/Pr%C3%A1ctica%203/Capturas/bench_pound.PNG)

Como se puede observar, tanto HAProxy como NGINX han obtenido tiempos de respuesta muy similares, siendo NGINX el más rápido en este caso. Por otro lado, Pound ha obtenido el peor tiempo.

























