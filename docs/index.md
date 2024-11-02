#  Práctica 2.4 – Balanceo de carga con proxy inverso en Nginx 

## IPs
Para esta práctica las IPs de mis servidores han sido:

- ip webserver1 192.168.43.47
- ip webserver2 192.168.43.46
- ip proxy 192.168.43.101

## Configuración de los webservers

Para ello primero debemos que cambiar el nombre de webserver por webserver1 en todos los sitios que aparezca. Para ello primer eliminamos el enlace simbólico en el servidor 1:

![imagen 1](assets/images/1.png)

Cambiamos el nombre de la carpeta del servidor y creamos la ruta /var/www/webserver1/html . Ahora esta será la ruta de nuestra pagina web

![imagen 2](assets/images/3.png)

Ahora debemos quitar la página web que descargamos en la práctica anterior
y remplazarlo por el siguiente html:

![imagen 3](assets/images/24.png)

Que para el server 1 quedaría:

![imagen 4](assets/images/4.png)

Cambiamos el nombre a los archivos de configuración:

![imagen 5](assets/images/5.png)

Ahora el paso más importante, el archivo de configuración. Solo tenemos que cambiar el nombre del servidor y de la cabecera a webserver1 y Serv_Web1_salva para la cabecera:

![imagen 6](assets/images/6.png)

Por último volvemos a crear el enlace simbólico:

![imagen 7](assets/images/7.png)

Reiniciamos el servicio y ahora debemos de clonar el webserver que acabamos de hacer pero eligiendo la opción de virtual box que nos da una nueva dirección MAC. Esto es para que el router le asigne una nueva IP. 

Repetimos todo el proceso de la misma forma:

![imagen 8](assets/images/8.png)

Creamos el html con el nombre bien puesto:

![imagen 9](assets/images/9.png)

Cambiamos nombre

![imagen 10](assets/images/10.png)

![imagen 11](assets/images/11.png)

Una vez hecho volvemos a crear el enlace simbólico

![imagen 12](assets/images/)

Ahora debemos de cambiar el archivo de configuración de la misma forma:

![imagen 13](assets/images/14.png)



## Configuración del balanceador de carga

## Cuestiones finales