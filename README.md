# Tutorial Apache Web

<span style="color:lime; font-size:17px"> **Buscad los archivos en el proyecto para usarlos de referencia en vuestras creaciones**

## Estructura de archivos

Creamos la siguiente estructura de archivos (solo lo nombrado a continuación):

- Creamos carpeta `conf` y copiamos archivos `conf` por defecto.

<img src="./images/carpetaConfDefault.png" alt="archivos" style="padding-left:40px; padding-bottom:30px">

- Creamos carpetas `sites-available`, copiamos archivos `conf` `default` y creamos archivos `conf` para cada webhost que vayamos a generar.

<img src="./images/carpetaSitesAvailable.png" alt="archivos" style="padding-left:40px; padding-bottom:30px">

- Creamos carpeta `websites` y dentro creamos una carpeta para cada webhost.

<img src="./images/carpetaWebsites.png" alt="archivos" style="padding-left:40px; padding-bottom:30px">

- Creamos `html` de web por defecto dentro de `websites` y `html` de cada web dentro de su carpeta.

<img src="./images/webhostDefault.png" alt="archivos" style="padding-left:40px; padding-bottom:30px">

- Creamos carpeta `errors` y dentro cada `html` de cada error que vayamos a controlar.

<img src="./images/carpetaErrors.png" alt="archivos" style="padding-left:40px; padding-bottom:10px">

<img src="./images/errorHtml.png" alt="archivos" style="padding-left:40px; padding-bottom:30px">

- Creamos carpeta `privado` dentro del webhost en el cual vamos a añadir el SSL con su `html`. A esta solo se accederá al iniciar sesión con el usuario autenticado. Además, crearemos el archivo `.htaccess` dentro.

<img src="./images/websitePrivado.png" alt="archivos" style="padding-left:40px; padding-bottom:30px">

- Creamos `script` con los comandos que realizará el contenedor automáticamente al iniciarse. En nuestro caso habilitaremos los sitios creados, habilitaremos el módulo ssl y reiniciaremos el servicio de `Apache` para aplicar los cambios.

<img src="./images/script.png" alt="archivos" style="padding-left:40px; padding-bottom:30px">


- Creamos carpeta `htpasswd` (el archivo se añadirá más tarde).

<img src="./images/carpetaHtpasswd.png" alt="archivos" style="padding-left:40px; padding-bottom:30px">

- Creamos carpeta `certs` (los archivos se añadirán más tarde).

<img src="./images/carpetaCerts.png" alt="archivos" style="padding-left:40px; padding-bottom:30px">

- Creamos `docker-compose.yml` con la configuración de nuestro contenedor.

<img src="./images/docker-compose.png" alt="archivos" style="padding-left:40px; padding-bottom:30px">


## Generación de certificados

Instalamos el programa **OpenSSL**.

Abrimos la terminal de OpenSSL y ejecutamos el siguiente comando (cambiar `seguro` por nombre de webhost):

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout seguro.key -out seguro.crt
```

Nos pedirá una serie de datos. Respondemos a los datos que pide y en el **Common Name** ponemos el nombre del webhost incluida la extensión (`seguro.net`).

![foto](./images/certificados.png)

Estos archivos se generarán en la ruta donde ejecutes el comando. Copia estos archivos a la carpeta `certs`.

## Modificación del archivo hosts

Modificamos el archivo `hosts` de nuestro PC para generar los DNS de nuestros webhosts.

Vamos a la ruta: `C:\Windows\System32\drivers\etc`

Dentro de esta habrá un archivo `hosts`. Haremos una copia de este como backup.

Modificamos el archivo `hosts` añadiendo las IPs y nombres de nuestros webhosts.

![foto](./images/archivoHosts.png)

## Lanzamiento del contenedor

Lanzamos nuestro `docker-compose.yml` con el comando:

```bash
docker-compose –build -d
```

Si tenemos algún error podemos borrar lo generado con el comando:

```bash
docker-compose down
```

Para acceder a la terminal del servidor generado usamos el comando:

```bash
docker exec -it apache_server /bin/bash
```

(También sirve `bash`).

![foto](./images/comandosDocker.png)

## Generación del archivo .htpasswd

Generamos archivo `.htpasswd` con el usuario y contraseña que deseemos.

Una vez dentro de la terminal de Apache, iremos a la ruta `/etc/apache2`.

Con el comando `ls -a` vemos los archivos ocultos y podemos ver que existe el archivo `.htpasswd`. En caso de no existir, lo generaremos nuevo, y si existe lo sobreescribiremos.

Generamos el archivo con el siguiente comando:

```bash
htpasswd -c /etc/apache2/.htpasswd usuario
```

En `usuario` pondremos el nombre del usuario que queramos, y la contraseña la introduciremos cuando nos la pida.

![foto](./images/htpasswd.png)

El archivo se copiará directamente en la ruta de nuestro proyecto al haber creado el volumen en el `docker-compose.yml`.

## Pruebas en el navegador

Ahora abrimos el navegador y buscamos las siguientes rutas:

- `http://jaime.com:8080`

<img src="./images/jaime.com.png" alt="archivos" style="padding-left:40px; padding-bottom:10px">

<img src="./images/jaime.com-error404.png" alt="archivos" style="padding-left:40px; padding-bottom:30px">

- `http://leon.com:8080`

<img src="./images/leon.com.png" alt="archivos" style="padding-left:40px; padding-bottom:30px">

- `https://seguro.net:8443`

<img src="./images/seguro.net.png" alt="archivos" style="padding-left:40px; padding-bottom:30px">

- `https://seguro.net/privado` ➡️ **Aquí introducimos los credenciales generados con htpasswd**

<img src="./images/loginPrivado.png" alt="archivos" style="padding-left:40px; padding-bottom:10px">

<img src="./images/seguro.net-privado.png" alt="archivos" style="padding-left:40px; padding-bottom:10px">

<img src="./images/seguro.net-error401.png" alt="archivos" style="padding-left:40px; padding-bottom:80px">

# Autor

Realizado por [Jaime León Mulero](https://github.com/jaimeleon10)

<img src="./images/fondoJaime.png" alt="archivos">
