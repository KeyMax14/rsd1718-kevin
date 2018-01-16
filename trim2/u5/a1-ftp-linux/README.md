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

En mi caso me voy a conectar a la máquina cliente y ejecutar el comando desde allí, desde aquí haremos copias desde el servidor al cliente y viceversa. La estructura es la misma que el comando `CP`.
```
scp 'ruta origen' 'ruta destino'
```
Y cuando nos conectemos a una máquina externa usamos la siguiente nomenclatura:

```
'usuario'@'host':'ruta remota'  # Por defecto la ruta del usuario es el home del mismo.
```

- Copiamos un archivo desde el home de `eric` a nuestro cliente.

  ![Imagen](img/023.png)

  ![Imagen](img/021.png)

- Copiamos un archivo desde el home de `stan` a nuestro cliente.

  ![Imagen](img/024.png)

  ![Imagen](img/022.png)

- Copiamos un archivo desde nuestro cliente al home de `eric` y al home de `stan`.

  ![Imagen](img/025.png)

  ![Imagen](img/026.png)

  ![Imagen](img/027.png)

## 7. Instalar el servicio `proftpd`

Ahora vamos a instalar el servicio `proftpd`, con este servicio podemos hacer que los usuarios puedan acceder a nuestro servidor mediante `ftp`, con `proftpd` podemos hacer algunas restricciones dentro de este servicio como veremos más adelante, para instalar el servicio ejecutaremos el comando `sudo apt install proftpd`

![Imagen](img/028.png)

A mitad de la instalación nos pedirá elegir entre `desde inetd` o `independiente` según el trafico que recibamos en nuestro servidor, en nuestro caso lo dejamos por defecto.

![Imagen](img/029.png)

### 7.1. Configurar `proftpd`

Podemos modificar todo lo referido a `proftpd` desde el fichero `/etc/proftpd/proftpd.conf`, también podemos añadir archivos de configuración separados en la subcarpeta `/etc/proftpd/conf.d`.

![Imagen](img/030.png)

Nosotros vamos a modificar en el fichero `proftpd.conf` descomentando la linea `DefaultRoot`, que es un parametro con el que podemos indicar el `home` de los usuarios, y el punto a partir del cual no pueden retroceder.

![Imagen](img/031.png)

> Configuramos al usuario `eric` en su home y al usuario `stan` desde la raíz, ya que es un usuario con permisos de administrador.

Reiniciamos el servicio para que se refresquen los cambios.

![Imagen](img/032.png)

### 7.2. Comprobaciones

Vamos a comprobar que funciona nuestro servicio FTP, lo primero que comprobaremos será acceder desde nuestro servidor.

![Imagen](img/033.png)

![Imagen](img/033-1.png)

> En esta última captura podemos observar que el usuario `eric` esta limitado solo a su home.

Ahora vamos a comprobar que se puede acceder desde el cliente.

![Imagen](img/034.png)

![Imagen](img/035.png)

> En este caso si es posible acceder a rutas anteriores a `/home/stan`, porque indicamos en el `DefaultRoot` de `stan` la ruta `/`.

Vamos a hacer algunas pruebas de subida y descarga de archivos utilizando nuestros dos usuarios desde el cliente:

- Usuario `eric`:

  - Descargamos un archivo desde `/home/eric` a nuestro cliente.

    ![Imagen](img/036.png)

    ![Imagen](img/037.png)

  - Subimos un archivo desde el cliente a `/home/eric` en remoto.

    ![Imagen](img/038.png)

    ![Imagen](img/039.png)

- Usuario `stan`:

  - Descargamos un archivo desde `/home/stan` a nuestro cliente.

    ![Imagen](img/040.png)

    ![Imagen](img/041.png)

    ![Imagen](img/042.png)

  - Subimos un archivo desde el cliente a `/home/stan` en remoto.

    ![Imagen](img/043.png)

Una vez hecho esto podemos dar por finalizada la actividad.
