# U5-A1 Servidor FTP Linux

![Imagen](img/000.png)

Esta es la continuación de la practica `U5-A1 Servidor FTP Windows`. En esta ocasión vamos a realizar la instalación y configuración del servicio FTP en una MV linux, concretamente con `ubuntu 16.04`.

## 1. Instalación del servicio SSH

Antes de pasar con un Servidor FTP al uso vamos a instalar `SSH`. Con este servicio seremos capaces de utilizar protocolos como `SFTP`. Para instalar el mismo ejecutaremos el comando `apt install ssh`.

![Imagen](img/001.png)

Una vez instalado el paquete podemos comprobar que esta en nuestro sistema ejecutando el comando `ssh -V`,  comando el cual nos mostrará la versión del programa instalado.

![Imagen](img/002.png)

## 2. Creación de clientes

Antes de comenzar a probar si funciona correctamente vamos a crear diferentes usuarios con los que loguearnos a `SSH`, uno de ellos tendrá rol de `administrador` y el otro será un usuario estándar.

![Imagen](img/004.png)

![Imagen](img/005.png)

![Imagen](img/006.png)

En las siguientes capturas se ven los diferentes tipos de privilegios.

![Imagen](img/007.png)

![Imagen](img/008.png)

## 3. Conexión usando `SSH`

Ahora vamos a probar la conexión `SSH` haciendo uso de una MV cliente, en mi caso otra máquina con `Ubuntu 16.04`. Probemos a loguearnos con el usuario `eric` haciendo uso del comando `ssh eric@172.18.21.81`, donde el final es la IP de mi servidor.

![Imagen](img/010.png)

También podemos intentar establecer una conexión con el usuario `stan`.

![Imagen](img/011.png)

## 4. Ejecutar un aplicación grá0fica con `SSH`

Con `SSH` también podemos utilizar aplicaciones graficas que se encuentren en la MV servidor desde el cliente si activamos `X Forwarding`. Para esto debemos modificar el fichero `/etc/ssh/sshd_config`.

![Imagen](img/012.png)

Dentro de este fichero debemos descomentar la línea `X11Forwarding yes` o escribirla nosotros mismos.

![Imagen](img/013.png)

Ahora podemos iniciar sesión desde un cliente mediante `SSH` a nuestro servidor y hacer uso de las aplicaciones gráficas si iniciamos sesión utilizando el comando `ssh -X eric@172.18.21.81`.

![Imagen](img/014.png)

Podemos comprobar que tenemos acceso a las aplicaciones gráficas ejecutando por ejemplo `gedit` desde la conexión `SSH`.

![Imagen](img/015.png)

## 5. Conexión mediante SFTP

Instalando `SSH` como indique al principio de la practica también te permite realizar conexiones `SFTP`. Probemos a conectarnos desde la MV cliente.

![Imagen](img/016.png)

> Haciendo uso del comando `get index.php` podemos comprobar que tenemos la posibilidad de descargar archivos.

Vamos a crear un archivo en nuestra MV cliente para comprobar que podemos subir archivos vía `SFTP`.

![Imagen](img/017.png)

Haciendo uso del comando `put subir-sftp.txt` podemos comprobar que también nos permite subir archivos.

![Imagen](img/018.png)

Esto también lo podemos hacer utilizando otro usuario, por ejemplo `stan`.

![Imagen](img/019.png)

![Imagen](img/020.png)

## 6. `SCP`

Otra herramienta de la que podemos hacer uso es de `SCP`, con esta herramienta podemos realizar copias seguras desde máquinas remotas.
