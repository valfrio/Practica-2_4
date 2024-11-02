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

![imagen 12](assets/images/12.png)

Ahora debemos de cambiar el archivo de configuración de la misma forma:

![imagen 13](assets/images/14.png)


## Configuración del balanceador de carga

Ahora debemos de cambiar el nombre de lso archivos de configuración por balanceo

![imagen 14](assets/images/15.png)

Ahora el paso clave. Debemos de cambiar el archivo de configuración para que el proxy-balanceador de carga redirija el tráfico a los dos servidores. Para ello debemos cambiar el archivo para que tenga este formato:

```
 upstream backend_hosts {
                random;
                server ________:____;
                server ________:____;
    }
            server {
                listen 80;
                server_name ________;      
                location / {
                    proxy_pass http://backend_hosts;
                }
            }
```

El bloque de server se encarga de redirigir el tráfico a los backend_hosts. Estos son el grupo de servidores definidos arriba en el bloque de upstream. Este bloque se encarga de la redirección y de que patrón seguir, en nuestro caso random para mayor simplicidad. Ahí añadiremos los servidores, donde la primera raya es donde tiene que estar la ip del servidor y en la siguiente raya después de los dos puntos el puerto que escucha el servidor. 

El archivo de configuración en mi caso quedó:

![imagen 15](assets/images/16.png)

Ahora entramos para comprobar que funciona

![imagen 16](assets/images/17.png)

Podemos ver que nos ha servido el primer servidor. Si recargamos las suficientes veces veremos que nos servirá el segundo servidor

![imagen 17](assets/images/18.png)

Podemos ver los logs de acceso para mayor seguridad:

![imagen 18](assets/images/19.png)

![imagen 19](assets/images/20.png)

Ahora veremos que el balanceador comprueba que los servicios estén activos a la hora de redirigir. Pararemos el servicio en ambas máquinas y veremos que efectivamente redirige a la otra todo el rato. Comenzamos con el primer servidor:

![imagen 20](assets/images/21.png)

Efectivamente me redirigió solo al servidor 2.

![imagen 21](assets/images/18.png)

Iniciamos de nuevo el servicio

![imagen 22](assets/images/22.png)

Y hacemos lo mismo con el webserver2

![imagen 23](assets/images/23.png)

![imagen 24](assets/images/17.png)


## Cuestiones finales

### Cuestión 1

Los siguientes métodos encontrados son los más comunes que he visto:

- Round Robin: Asigna solicitudes secuencialmente entre los servidores.

- Least Connections: Envía la solicitud al servidor con menos conexiones activas.

- IP Hash: Utiliza la IP del cliente para definir siempre el mismo servidor en función de su dirección

### Cuestión 2


El bloque de server se encarga de redirigir el tráfico a los backend_hosts. Estos son el grupo de servidores definidos arriba en el bloque de upstream. Este bloque se encarga de la redirección y de que patrón seguir. Ahí añadiremos los servidores, donde la primera raya es donde tiene que estar la ip del servidor y en la siguiente raya después de los dos puntos el puerto que escucha el servidor. 

### Cuestión 3

Deberíamos seguir dos pasos:

- Definir el bloque upstream  para añadir la ip y los puertos de las peticiones, así añadiendo la función de balanceador.

- Definir en el bloque location el proxy_pass que redirija las peticiones a los servidores definidos en el upstream.