# Tema 2
##### Servidores Web de Altas Prestaciones - Dpto. de Arquitectura y Tecnología de Computadores

**Ejercicio 1**  

A partir de las disponibilidades dadas, para calcular la disponibilidad final debemos usar la siguiente fórmula *As = ACn-1 + ( (1 - ACn-1)· ACn)*

| Elemento     | 3 Elementos   |
| ------------ | ------------- |
| Web          | 99,6625%      |
| Application  | 99,9%         |
| Database     | 99,9999999%   |
| DNS          | 99,9992%      |
| Firewall     | 99,6625%      |
| Switch       | 99,9999%      |
| Data Center  | 99,999999%    |
| ISP          | 99,9875%      |

Disponibilidad total = 99,6625 · 99,9 · 99,9999999 · 99,9992 · 99,6625 · 99,9999 · 99,99999 · 99,9875 = 99,2135%  
  
**Ejercicio 2**
Django (Python), Jquery (Javascript), AngularJS(JavaScript), Docker.
  
  
  
**Ejercicio 3**  

Tal y como hemos procedido en las prácticas de la asignatura, Apache Benchmark junto con Htop nos permitía observar como se comportaba nuestra granja web ante el incremento de carga.
Otra herramienta bastante útil para esta finalidad es Zabbix.  
  
  
**Ejercicio 4**
  
Balanceadores Software: Haproxy, ZenLoadBalancer, Pound, Nginx, etc.
Balanceadores Hardware: CISCO, Coyote Point, Kemp load Balancer, etc.

