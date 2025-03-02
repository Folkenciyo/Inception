# Inception

Este proyecto de la escuela 42 tiene como objetivo ampliar tus conocimientos de administración de sistemas utilizando Docker. En este tutorial, virtualizarás varias imágenes de Docker, creándolas en tu nueva máquina virtual personal. En este README tendrás un tutorial de Inception para saber cómo funciona el proyecto.

## Cosas importantes que leer antes de comenzar el proyecto

1. **No intentes hacer todos los contenedores**  (Nginx, Wordpress y MariaDB) al mismo tiempo.
Vas a perderte y no entenderás bien cómo funciona. Hazlo paso a paso.

2. **Comienza con Nginx** mostrando una página index.html
   - Aprende primero cómo lanzar una imagen de Docker && ejecutar esta imagen sin usar docker-compose
   - Aprende cómo mostrar una página html en http://localhost:80
   - Aprende cómo mostrar una página html con SSL en http://localhost:443

3. **Haz Wordpress**
	- Puedes comenzar aquí con el archivo docker-compose, no lo necesitas antes

4. **Termina con MariaDB.**

¿Quieres probar si cada contenedor funciona en general? No te preocupes, podrás hacerlo importando imágenes para Wordpress y MariaDB desde el hub. (Si lees esto por primera vez, te invito a comenzar a leer este excelente README y ¡ponerle una estrella! Ayuda!)

- Los tres repositorios de GitHub que me ayudaron mucho con el proyecto: [vbachele](https://github.com/vbachele/Inception/tree/main), [llescure](https://github.com/llescure/42_Inception) y [malatini](https://github.com/42cursus/inception)
- Este GitHub me ayudó con el bonus [twagger](https://github.com/twagger/inception)  

Si tienes preguntas: por favor contáctame, estaré encantado de darte una respuesta. Mi nombre de usuario en Discord: folkenciyo

# Resumen

### 1. [DEFINICIONES](https://github.com/Folkenciyo/Inception/blob/main/README.md#definitions)
### 2. [DOCKER](https://github.com/Folkenciyo/Inception/blob/main/README.md#Docker)
### 3. [NGINX](https://github.com/Folkenciyo/Inception/blob/main/README.md#NGINX)
### 4. [WORDPRESS](https://github.com/Folkenciyo/Inception/blob/main/README.md#WORDPRESS)
### 4. [MARIADB](https://github.com/Folkenciyo/Inception/blob/main/README.md#MARIADB)
### 5. [BONUS](https://github.com/Folkenciyo/Inception/blob/main/README.md#BONUS)
- [REDIS](https://github.com/Folkenciyo/Inception/blob/main/README.md#REDIS)
- [FTP-server](https://github.com/Folkenciyo/Inception/blob/main/README.md#FTP-SERVER)
- [Adminer](https://github.com/Folkenciyo/Inception/blob/main/README.md#ADMINER)
- [Service of my choice (hugo)](https://github.com/Folkenciyo/Inception/blob/main/README.md#Service-of-my-choice)
- [Static web page](https://github.com/Folkenciyo/Inception/blob/main/README.md#Static-web-page)

# Definiciones
## ¿Qué es Docker?
Docker es una plataforma abierta para desarrollar, enviar y ejecutar aplicaciones. Docker te permite separar tus aplicaciones de tu infraestructura para entregar software rápidamente. Con Docker, puedes gestionar tu infraestructura de la misma manera que gestionas tus aplicaciones. Al aprovechar las metodologías de Docker para enviar, probar y desplegar código rápidamente, puedes reducir significativamente el retraso entre escribir código y ejecutarlo en producción.
Docker proporciona la capacidad de empaquetar y ejecutar una aplicación en un entorno ligeramente aislado llamado contenedor.

## ¿Qué es docker-compose?
- [¿Qué es Docker en general?](https://www.educative.io/blog/docker-compose-tutorial)

- [¿Qué es la red de Docker?](https://www.aquasec.com/cloud-native-academy/docker-container/docker-networking/)

Compose es una herramienta para definir y ejecutar aplicaciones de Docker multi-contenedor. Con Compose, usas un archivo YAML para configurar los servicios de tu aplicación. Entonces, con un solo comando, creas y empiezas todos los servicios desde tu configuración.

## ¿Qué es un archivo Dockerfile?
Docker puede construir imágenes automáticamente leyendo las instrucciones desde un Dockerfile. Un Dockerfile es un documento de texto que contiene todas las instrucciones que un usuario podría llamar en la línea de comandos para ensamblar una imagen. Usando ```docker build```, los usuarios pueden crear una compilación automatizada que ejecute varias instrucciones de la línea de comandos en sucesión.

## Cómo instalar Docker en MacOS
Para este proyecto, estoy en mi Mac personal, así que no necesito usar la máquina virtual para usar el comando sudo.
Tuve que instalar Docker. Primero:

- Fui directamente al sitio web de Docker y descargué Docker [Docker Docs](https://docs.docker.com/desktop/)
- Instalé Docker en la máquina
- Probé ejecutar un Dockerfile gracias al comando ```docker run hello-world```

## Cosas útiles que debes saber sobre los dockers e inception containers
- En Mac, el servicio Apache está instalado por defecto. Elimine Apache de la computadora para evitar cualquier problema con nginx, en caso dde tener Linux no debería de estar instalado.
- Si estás en 42 en su computadora, deberías detener estos servicios que se están ejecutando por defecto, por si acaso.
```c
sudo service nginx stop
sudo service mariadb stop
sudo service apache2 stop
sudo service mysql stop
```

# DOCKER

## Comandos importantes para usar Docker
- [Prácticas recomendadas para construir contenedores](https://cloud.google.com/architecture/best-practices-for-building-containers)

### Comandos generales de Docker
```c
- docker ps or docker ps -a //show the names of all the containers you have + the id you need and the port associated.
- docker pull "NameOfTheImage" // pull an image from dockerhub
- docker "Three first letter of your docker" // show the logs of your last run of dockers
- docker rm $(docker ps -a -q) //allow to delete all the opened images
- docker exec -it "Three first letter of your docker" sh // to execute the program with the shell
```

### Docker run

```c
- docker run "name of the docker image" //to run the docker image
- docker run -d, // run container in background
- docker run -p,// publish a container's port to the host
- docker run -P, // publish all exposed port to random ports
- docker run -it "imageName", //le programme continuera de fonctionner et on pourra interagir avec le container
- docker run -name sl mysql, //give a name for the container instead an ID
- docker run -d -p 7000:80 test:latest
```

### Docker image
```c
- docker image rm -f "image name/id", //delete the image, if the image is running you need to kill it first.
- docker image kill "name", //stop a running image,
```

## Cómo escribir un archivo Dockerfile
- Crea un archivo llamado dockerfile
- Escribe tus comandos dentro del doc
- Construye el dockerfile con el comando "docker build -t "nombreQueEligas"."
- Ejecuta el dockerfile con el comando: docker run "nombreQueEligas"

Aquí están los tipos de instrucciones más comunes:

- **FROM** \<image\> define una base para tu imagen. ejemplo : FROM debian
- **RUN** \<command\> ejecuta cualquier comando en una nueva capa encima de la imagen actual y compromete el resultado. RUN también tiene una forma de shell para ejecutar comandos.
- **WORKDIR** \<directory\> establece el directorio de trabajo para cualquier RUN, CMD, ENTRYPOINT, COPY y ADD que sigan en el Dockerfile. (Vas directamente al directorio que elijas)
- **COPY** \<src\> \<dest\> copia nuevos archivos o directorios desde \<src\> y los agrega al sistema de archivos del contenedor en la ruta \<dest\>.
- **CMD** \<command\> te permite definir el programa predeterminado que se ejecutará una vez que inicies el contenedor basado en esta imagen. Cada Dockerfile solo tiene un CMD, y solo se respeta la última instancia de CMD cuando existen múltiples.

## Cómo lanzar una página web local para probar
### **(este punto funciona solo en Mac y no en la VM)**
### [Video tutorial](<https://www.youtube.com/watch?v=F2il_Mo5yww&ab_channel=linuxxraza>)
- Crea un archivo HTML con algún código en él.
- Crea tu archivo dockerfile
   - La imagen será NGINX : FROM NGINX
   - Usa COPY para copiar tus archivos en el directorio html de NGINX
- Usa el comando "docker build -t simple ."
- Usa el comando "docker container run --name="nombrede tu elección" -d -p 9000:80 simple"
   - --name es para dar un nombre a tu imagen
   - -d ejecuta el contenedor en segundo plano
   - -p publica el puerto del contenedor en el host. En este caso 9000 a 80

# NGINX

## Cómo configurar NGINX (nuestro servidor web)
- [Video tutorial](<http://nginx.org/en/docs/beginners_guide.html>)

Nginx es un servidor web que almacena archivos hmtl, js, imágenes y usa solicitudes http para mostrar un sitio web.
Los documentos de configuración de Nginx se utilizarán para configurar nuestro servidor y la conexión proxy correcta.

## Configurar el archivo .conf en nginx
### Enlaces útiles de nginx
- [explicaciones de ubicación](<https://www.digitalocean.com/community/tutorials/nginx-location-directive>)
- [Qué es un servidor proxy](<https://www.varonis.com/fr/blog/serveur-proxy>)
- [Todas las definiciones de nginx](<http://nginx.org/en/docs/http/ngx_http_core_module.html>)
- [Línea de comandos de Nginx](<https://www.nginx.com/resources/wiki/start/topics/tutorials/commandline/>)
- [Manejo de señales PID 1 y nginx](https://cloud.google.com/architecture/best-practices-for-building-containers#signal-handling)
- [Qué es TLS (en francés)](https://fr.wikipedia.org/wiki/Transport_Layer_Security)

### Listen && Location
- Listen indicará al servidor qué solicitudes debe aceptar: 
- Listen puede tomar puertos y direcciones: ejemplo Listen 80;

- La directiva location dentro del bloque de servidor NGINX permite enrutar solicitudes al lugar correcto dentro del sistema de archivos.
- La directiva se utiliza para decirle a NGINX dónde buscar un recurso incluyendo archivos y carpetas mientras compara un bloque de ubicación contra una URL.

## Pasos para agregar en localhost configurando 
### **(este punto funciona solo en Local y no en la VM)**
1. Agregué un archivo index.html en mi directorio /var/www/
2. Configuré el archivo predeterminado en etc/nginx/site-enabled/default
3. Agregué un corchete de servidor con una ubicación hacia var/www/ en el documento. Guárdalo y recarga nginx con 'nginx -s reload'.
4. Porque el puerto host que puse cuando construí era 7000. Ve a una página web y pon: http://localhost:7000/ . ¡¡¡¡¡Funciona!!!!

[IMAGEN]![nginxLocalImage](images/nginxLocalImages.png)

## Cómo cambiar tu localhost por juguerre.42.fr
1. Ve al archivo /etc/hosts
2. Añade la siguiente línea: "127.0.0.1 juguerre.42.fr"

## Fastcgi (o cómo procesar PHP con nginx)
### Enlaces útiles
- [Qué es http](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)
- [diferencia entre http y tcp](https://www.goanywhere.com/blog/http-vs-tcp-whats-the-difference#:~:text=TCP%20contains%20information%20about%20what,data%20in%20the%20stream%20contains.)
- [Ejemplos de PHP Fast CGI](https://www.nginx.com/resources/wiki/start/topics/examples/phpfcgi/)
- [Por qué usar fastcgi_pass 127.0.0.1:9000](https://serverfault.com/questions/1094793/what-is-this-nginx-location-for-php-fpm-fastcgi-pass-127-0-0-19000-really-doing)
- [Instalar Nginx con php-fpm en video](https://www.youtube.com/watch?v=I_9-xWmkh28&ab_channel=ProgramWithGio)
- [Explicaciones de comandos Fast CGI](https://www.digitalocean.com/community/tutorials/understanding-and-implementing-fastcgi-proxying-in-nginx)

PHP-FPM (por fast-cgi Process Manager) se ejecuta como un servicio aislado cuando usas PHP-FPM.
Emplear esta versión de PHP como el intérprete de lenguaje significa que las solicitudes se procesarán a través de un socket TCP/IP,
y el servidor Nginx maneja solo las solicitudes HTTP, mientras que PHP-FPM interpreta el código PHP. Aprovechar dos servicios separados es vital para volverse más eficiente.
Se presenta con Wordpress

# Docker-compose
- [tutorial open classroom dockercompose](https://openclassrooms.com/fr/courses/2035766-optimisez-votre-deploiement-en-creant-des-conteneurs-avec-docker/6211624-decouvrez-et-installez-docker-compose)

## Docker-Compose commands
```c
- docker-compose up -d --build, //Create and build all the containers and they still run in the background
- docker-compose ps, //Check the status for all the containers
- docker-compose logs -f --tail 5, //see the first 5 lines of the logs of your containers
- docker-compose stop , //stop a stack of your docker compose
- Docker-compose down, //destroy all your ressources
- docker-compose config, //check the syntax of you docker-compose file
```

## Inside the docker-compose file
Toda la información sobre lo que significa cada línea está en este [tutorial](https://openclassrooms.com/fr/courses/2035766-optimisez-votre-deploiement-en-creant-des-conteneurs-avec-docker/6211677-creez-un-fichier-docker-compose-pour-orchestrer-vos-conteneurs)

# WORDPRESS 
## Useful links
- [¿Qué es la CLI de WordPress?](https://www.dreamhost.com/wordpress/guide-to-wp-cli/#:~:text=The%20WP%2DCLI%20is%20a,faster%20using%20the%20WP%2DCLI.)  
- [Saber más sobre wp-config.php](https://wpformation.com/wp-config-php-et-functions-php-fichiers-wordpress/)  
- [php-fpm - www.conf](https://myjeeva.com/php-fpm-configuration-101.html)  

*definitions*
*wp-config.php* Este archivo le dice a tu base de datos cómo obtener tus archivos y cómo tratarlos
## ¿Cuáles son los pasos para crear tu WordPress?
1. **Crea tu imagen de dockerfile**
    - Descargar php-fpm
    - Copiar el archivo www.conf en php/7.3/fpm/pool.d/
    - Crear el directorio php para habilitar la ejecución de php-fpm
    - Copiar el script y ejecutarlo
    - Ir al directorio html
    - Ejecutar php-fpm

2. **Crear un script**
    - Descargar wordpress
    - Crear el archivo de configuración de wordpress
    - Mover los archivos de wordpress al directorio html
    - Proporcionar las 4 variables de entorno para wordpress

3. **Crear un www.conf**
Necesitas editar www.conf y colocarlo en /etc/php/7.3 (la versión usual de php en la vm de 42) /fpm/pool.d y wp-content.php para deshabilitar el acceso a la página de instalación de wordpress cuando accedas a tu sitio en https://login.42.fr
    - Poner listen = 0.0.0.0:9000 para escuchar en todos los puertos
    - Incrementar el número de los valores pm para evitar una página 502

# MARIADB
MariaDB será la base de datos para almacenar información sobre nuestros usuarios de WordPress y ajustes.
En esta sección tenemos que crear la imagen de MariaDB y crear 2 usuarios.

## Useful links
- [Importar-exportar bases de datos](https://www.interserver.net/tips/kb/import-export-databases-mysql-command-line/)  
- [Crear y otorgar permisos a un usuario](https://www.daniloaz.com/en/how-to-create-a-user-in-mysql-mariadb-and-grant-permissions-on-a-specific-database/)  
- [Por qué crear el directorio /var/run/mysqld](http://cactogeek.free.fr/autres/DocumentationLinux-Windows/LinuxUbuntu/ProblemeMYSQL-mysqld.sockInexistant.pdf)  
- [Cómo otorgar todos los privilegios a un usuario en una base de datos](https://chartio.com/resources/tutorials/how-to-grant-all-privileges-on-a-database-in-mysql/)  
- [Cómo importar una base de datos](https://www.journaldunet.fr/web-tech/developpement/1202663-comment-importer-un-fichier-sql-dans-mysql-en-ligne-de-commande/)

## MARIADB comandos útiles
```c
mysql -uroot // To connect on mysql CLI
SELECT User FROM mysql.user; // To see all the users
USE wordpress // To connect on your wordpress database
mysqldump -u username -p databasename > filename.sql // To export the file
mysql -uroot -p$MYSQL_ROOT_PASSWORD $MYSQL_DATABASE < /usr/local/bin/wordpress.sql // To import the file
```

## ¿Cuáles son los pasos para crear tu propia imagen de Maria DB?
1. **Crear un Dockerfile**
    - Descargar mariadb-server y mariadb-client
    - Para ejecutar MariaDB en tu contenedor, debes copiar tu .sh y el .sql en /var/local/bin/
    - Dar los permisos adecuados para ejecutar tu mysqld (que es el demonio para MySQL)
    - Ejecutar tu script para instalar MariaDB
    - Luego hacer un CMD para habilitar la base de datos para escuchar en todas las direcciones IPV4.

2. **Crear un script (archivo .sh)**
    - mysql_install_db inicializa el directorio de datos de MySQL y crea las tablas del sistema que contiene, si no existen
    - En este script descargamos MariaDB en el contenedor, debemos instalarlo y crear el usuario root
    - Luego ejecutamos la línea de comandos para otorgar todos los privilegios al usuario root. La función GRANT de mysqlcli (línea de comandos SQL) otorga acceso (o todo el acceso) a un usuario.

3. **Crear tu archivo.sql**
    - 2 opciones:
        1. Creas la base de datos, el usuario y le das todos los privilegios al usuario
            como [malatini hizo](https://github.com/42cursus/inception/blob/validated/srcs/requirements/mariadb/config/create_db.sql)
        2. Exportas tu propio wordpress.sql como yo hice (¡y Lea también lo hizo!)
            - Paso 1: Crear tu usuario administrador en WordPress:
                Quizás no sepas qué es, ¡no hay problema! Significa que exportarás tu usuario administrador desde tu base de datos para ponerlo en tu archivo .sql.
                - Ve a tu sitio web de WordPress (localhost:443) y crea tu usuario usando el mismo nombre de usuario y contraseña que tu archivo .env.
            - Paso 2: Exportar tu admin user.sql
                Debes ir a tu contenedor de MariaDB y ejecutar el siguiente comando
                - mysqldump -u 'nombredeusuario' -p 'nombredelabasededatos' > nombredearchivo.sql *"esto exportará tu usuario en el nombredearchivo.sql, por favor cambia nombredeusuario, nombredelabasededatos por lo que pusiste en tu archivo .env"*
                - Tienes un archivo llamado nombredearchivo.sql en tu directorio actual
                - "cat nombredearchivo.sql" en tu contenedor y copia y pega en tu proyecto .sql.
                - Tu .sql está listo ahora para ser importado
            - Paso 3: reiniciar tu docker-compose
                - TADA, estarás directamente en tu sitio web pasando la fase de instalación

[IMAGEN]![Wordpress without installation](images/wordpress_page.png)

### Comandos para verificar si todo funciona
```c
	SHOW DATABASES; // show the databes
	use 'wordpress'; // go in the wordpress databse
	SHOW TABLES; // show all the tables from the database you selected
	SELECT wp_users.display_name FROM wp_users; // display username from wordpress database
	SELECT *  FROM wp_users; // select
```

# BONUS

## REDIS
### Enlaces útiles
- [Qué es Redis, cómo funciona con WordPress y qué es un caché](https://www.section.io/engineering-education/how-to-set-up-and-configure-redis-caching-for-wordpress/)  
- [Cómo configurar Redis (artículo en inglés)](https://www.vultr.com/docshow-to-setup-redis-caching-for-wordpress-with-ubuntu-20-04-and-nginx/)  
- [Cómo configurar Redis (artículo en francés)](https://gaelbillon.com/installer-et-configurer-redis-pour-wordpress-en-5-minutes/)  

### Definición
Remote Dictionary Server (Redis) es una base de datos en memoria, persistente, de clave-valor conocida como un servidor de estructuras de datos. Un factor importante que diferencia a Redis de servidores similares es su capacidad para almacenar y manipular tipos de datos de alto nivel (ejemplos comunes incluyen listas, mapas, conjuntos y conjuntos ordenados).

### REDIS useful commands
```c
redis-cli // to connect with the cli
redis-server --protected-mode no // To set up redis when you launch your image
```

### Cómo configurar REDIS
1. **Crear un Dockerfile**
    - Instalar redis en él
    - Copiar el .sh en tu imagen
    - Ejecutar el .sh

2. **Crear un script shell**
*Redis por defecto tiene un redis.conf y necesitamos modificar 3 valores en él*
    - Modificar el valor en el documento .conf con la función sed
    - Ejecutar el comando redis-server para instalarlo

3. **Modificar el Dockerfile de WordPress**
    - Necesitas descargar wp-cli y moverlo al directorio de la aplicación (/usr/bin/wordpress)
    - Añadir la instalación de redis y php-redis

4. **Modificar tu script en el archivo de WordPress**  
*Para hacer esto, podemos establecer directamente la información en el script para el comando wpcli de WordPress*
- Modificar el archivo wp-config.php
    - Definir el Host de redis
    - Definir el Puerto de redis // Para redirigir el puerto de WordPress a este puerto
    - Definir la clave de caché de wp
    - Definir la contraseña de redis de wp
    - Definir el cliente de redis de wp
- Instalar el plugin de caché de redis, actualizarlo y habilitarlo

### Cómo saber si tu redis está instalado en WordPress y funcionando  
1. **Verificar que redis está correctamente instalado en tu imagen de redis**  
- Ejecuta el comando 'redis-cli -h localhost' en tu imagen de redis, deberías conectarte a tu localhost. Luego haz ping y la respuesta debería ser PONG. Genial, tu redis está instalado.

2. **Verificar si el plugin está instalado en WordPress**
    - Ve a tu panel de administración de WordPress: para mí es https://juguerre.42.fr
    - Haz clic en plugins en la pestaña de la izquierda
    - Si ves "Redis Object Cache", ¡Felicidades!, haz clic en configuraciones y verás el Estado "Conectado" en verde

## FTP SERVER

### Enlaces útiles
*Este es el más difícil de los bonus para hacer*
- [¿Qué es un servidor FTP?](https://titanftp.com/2022/07/05/what-is-an-ftp-server/)  
- [¿Qué es vsftpd?](https://en.wikipedia.org/wiki/Vsftpd)  
- [Instalar un servidor FTP con WordPress](http://praveen.kumar.in/2009/05/31setting-up-ftps-using-vsftpd-for-wordpress-plugins-auto-upgrade/)  
- [Entender el archivo vstftpd.conf (versión en francés)](https://linux.developpez.com/vsftpd/)  

### Definición
Un servidor FTP, en la definición más simple, es una aplicación de software que permite la transferencia de archivos de una computadora a otra. FTP (que significa "Protocolo de Transferencia de Archivos") es una forma de transferir archivos a cualquier computadora en el mundo que esté conectada a Internet. Para WordPress, permite modificar fácilmente tus archivos como los archivos de WordPress o tu código.

### ¿Cómo configurar tu servidor FTP?
1. **Crear un Dockerfile**
    - Descargar vsftpd (un servidor FTP seguro)
    - Copiar el archivo .conf en tu imagen FTP
    - Ejecutar tu script para instalar el servidor FTP
    - Ejecutar tu servidor FTP
2. **Modificar tu docker-compose.yml**
    - Crear tu imagen como de costumbre
    - Añadir los puertos 20 y 21 (puertos por defecto para servidores FTP)
    - Poner los mismos volúmenes que tu WordPress y nginx

3. **Crear un script para ejecutar**
*En este script crearemos un usuario, le daremos derechos*
    - Crear un usuario
    - Darle derechos a tus archivos www

4. **Crear archivo .conf**
*En este archivo .conf tendrás que configurar tu archivo para permitir el localhost*

### ¿Cómo saber si funciona?
Hice la prueba en mi macOS (debería funcionar en todas partes), tienes que descargar FileZilla, por ejemplo, es un cliente FTP que se comunicará con nuestro servidor vsftpd. Una vez instalado, como muestra la imagen a continuación, deberías ver el directorio que pusiste en tu archivo .conf con la línea *local_root=/var/www/html*. Si agregas un archivo en el directorio /var/www/html desde FileZilla, deberías poder verlo en tu contenedor nginx, wordpress o ftp-server.

![cliente filezilla](./images/ftp_server_filezilla.png)

## ADMINER

### Definición
Reemplaza phpMyAdmin con Adminer y obtendrás una interfaz de usuario más ordenada, mejor soporte para características de MySQL, mayor rendimiento y más seguridad.

### Cómo configurar adminer  
1. **Crear un Dockerfile**
    - Descargar curl y php
    - Descargar la versión de adminer
    - Mover el archivo php de adminer al archivo index.php (ubicado en var/www/html)
    - Añadir el usuario www-data
    - Mover tu archivo de configuración al directorio php-fpm.d

2. **Crear un archivo www.conf**
    - Necesitas añadir el puerto de escucha (*en mi caso el 9000*)
    - Añadir el propietario de escucha y el grupo de escucha *(en mi caso: www-data)*

3. **Modificar el archivo nginx.conf**
    - Necesitas añadir en tu nginx.conf una regla para escuchar adminer en el puerto 9000.
    - Verificará si el archivo index.php existe

4. **Modificar tu docker-compose.yml**
    - Crear como de costumbre una imagen docker de adminer en tu docker-compose

### ¿Cómo saber si adminer está funcionando?  
- Tienes que poner: https://"nombre_de_tu_sitio_web"/adminer *en mi caso https://juguerre.42.fr/adminer*
- Deberías ser redirigido a la página de conexión de adminer

## Servicio de mi elección 
### Enlaces útiles
- [Qué es hugo](https://gohugo.io/about/what-is-hugo/)  
- [Cómo configurar hugo](https://gohugo.io/getting-started/quick-start/)  
- [Configurar hugo (más explicaciones sobre el archivo .toml)](https://gohugo.io/getting-started/configuration/#configure-build)  

### Definition
Hugo is a fast and modern static site generator written in Go, and designed to make website creation fun again.

### How to set up hugo?
1. **Modify the docker-compose file**

2. **Create a dockerfile**
	- Download hugo
	- Create and go the dedicate directory (for me the name is *me*)
	- Create your webiste and add your template
	- Copy your toml file to replace the one for your directory

3. **Add a config.toml file**
	- The toml file is used by hugo as a configuration file
	- You need to add the #baseURL with your url
	- Your need to add the theme your downloaded on the dockerfile for my case it is "m10c"

4. **Modify the .nginx file**
	- You have to add a rule to listen the dedicated directory 
	- Add the rule for the proxy pass to listen your container
	- include the params for the proxy for nginx

### How to test your program?
	- You need to go on the URL you passed (my url is *https://juguerre.42.fr/me*)
	- If you see only a blank page, that means your theme is not applied it works but you need to find how to apply the theme.
	- If you see a page it works !
![hugo_website](images/hugo.png)

## Static web page
*For this one it is easy, I took the hugo services and I did the following changes.*
1. **Update the .toml file**
	- I changed the .toml file with my theme from the previous bonus to add a page about, presentation and my github link.

2. **Create a dockerfile**

3. **Update your docker-compose.yml**

4. **Update the .nginx conf file**
	- You need to update the .conf file to listen your new image in order to display the website.

4. **Create your static pages**
	- I created the about page in markdown
	- I created the presentation page in markdown

**Here is the website**
![static web page](images/static_page.png)