# ExamenSI

## Introducción

En este ejercicio realizaremos el despliegue de la aplicación  "LoginWebApp" mediante un docker-compose.yml
Desplegamos esta aplicación ya que el proyecto realizado por mi grupo la base de datos esta hecha con oracle y no es posible realizar un docker-compose que nos coja el volumen que contiene los datos.
Después subiremos a DockerHub una imagen para poder ser descargada posteriormente y ejecutada en cualquier otro equipo.

## Docker Compose

En primer lugar nos hemos descargado el repositorio que contiene los archivos de la aplicación a desplegar, los descomprimimos y los ubicamos en una carpeta.

Seguidamente en Visual Studio nos vamos a la carpeta que contiene todos los ficheros y creamos un nuevo docker-compose.yml

![1](https://github.com/cosmetorandellborras/ExamenSI/blob/main/1.png)

En primer lugar empezariamos a rellenar nuestro archivo docker-compose.yml

### Declaración contenedor database:
1. Declarar una imagen de mysql:5.7
2. Despues declaramos los volumenes de la base de datos
3. Declarar los credenciales que usara la base de datos
4. Declarar que puertos usará

![2](https://github.com/cosmetorandellborras/ExamenSI/blob/main/2.png)

### Declaración contenedor phpmyadmin:
1. Declaramos que el contenedor depende del servicio 'db'
2. Declaramos una imagen phpmyadmin
3. Declaramos los puertos que usara
4. Declaramos los credenciales que phpmyadmin para conectarse con la base de datos

![3](https://github.com/cosmetorandellborras/ExamenSI/blob/main/3.png)

### Declaración contenedor web:
1. Hacemos referencia al dockerfile personalizado del contenedor de tomcat
2. Declaramos que dependera del servicio db
3. Declaramos la imagen de tomcat
4. Declaramos que nos coja el .war que esta en la carpeta target y lo incluya dentro del contenedor de tomcat en el directorio webapps
5. Declaramos los puertos del contenedor tomcat
6. Declaramos de nuevo los credenciales de la base de datos
7. Finalmente Declaramos que monte los volumenes.

![4](https://github.com/cosmetorandellborras/ExamenSI/blob/main/4.png)

Con todo esto ya tendriamos nuestro docker-compose listo, pero como hacemos referencia a un dockerfile, antes de hacer un docker-compose up, tenemos que definir un docker file

## Docker File

En Visual Studio, creamos un nuevo fichero Dockerfile y lo cumplimentamos de la siguiente manera:

1. Empezamos indicando la imagen inicial con la etiqueta FROM, en este caso una imagen tomcat
2. Indicamos el mantenedor de la imagen mediante la etiqueta LABEL
3. Añadimos el fichero LoginWebApp de la carpeta target al directorio webapps de tomcat con la etiqueta ADD
4. Indicamos el puerto, con la etiqueta EXPOSE
5. Por ultimo indicamos el CMD

![5](https://github.com/cosmetorandellborras/ExamenSI/blob/main/5.png)

## Ejecucion docker-compose

Para ello en nuestra terminal, nos desplazamos hasta el directorio donde tenemos localizado el proyecto y los ficheros docker-compose y dockerfile y ejecutamos el siguiente comando:
~~~
docker compose up
~~~
![6](https://github.com/cosmetorandellborras/ExamenSI/blob/main/6.png)
Ahora ya tenemos nuestra aplicación desplegada en nuestro equipo, y podemos acceder tanto a la página web como al phpmyadmin

Web:
![7](https://github.com/cosmetorandellborras/ExamenSI/blob/main/7.png)
Phpmyadmin:
![8](https://github.com/cosmetorandellborras/ExamenSI/blob/main/8.png)

Para evitar un error he modificado la base de datos y agregado el campo 'regdate'
![9](https://github.com/cosmetorandellborras/ExamenSI/blob/main/9.png)

Ya es posible registrarse
![10](https://github.com/cosmetorandellborras/ExamenSI/blob/main/10.png)
![11](https://github.com/cosmetorandellborras/ExamenSI/blob/main/11.png)

## Preparación para subir la imagen a docker hub

En primer lugar tenemos que tener el fichero dockerfile ya listo, como lo hemos hecho en el paso anterior.

Seguidamente tenemos que hacer docker login, esto hara que que se validen nuestros credenciales.
![12](https://github.com/cosmetorandellborras/ExamenSI/blob/main/12.png)

Ahora desde Visual Studio haremos un docker build que nos creara la imagen personalizada, para ello en el fichero dockerfile, hacemos click derecho y le damos a la opción Build Image, seguidamente le indicamos el nombre de la imagen a crear
![13](https://github.com/cosmetorandellborras/ExamenSI/blob/main/13.png)

Finalmente vamos a la extensión de Docker de Visual Studio, buscamos nuestra imagen personalizada, hacemos click derecho y le damos a la opción push, esto nos subira nuestra imagen personalizada a Docker Hub
![14](https://github.com/cosmetorandellborras/ExamenSI/blob/main/14.png)

## Comprobación de que la imagen se ha subido con éxito

Para ello, nos dirigimos a Docker Hub, y buscamos en nuestro usuario si se ha subido la imagen que hemos creado.
Como podemos comprobar en la captura, encontramos la imagen sin problema
![15](https://github.com/cosmetorandellborras/ExamenSI/blob/main/15.png)
Ahora cualquier usuario puede hacer un pull de la imagen y desplegar la aplicación

### Enlace Docker Hub
[Enlace imagen Docker Hub](https://hub.docker.com/r/trui20/examensistemas)

