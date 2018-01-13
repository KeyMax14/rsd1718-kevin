# U5-A1 Servidor FTP Windows

![Imagen](img/000.png)

En esta practica vamos a instalar y configurar el servicio `FTP` en una máquina con **Windows server 2012**. En esta ocasión no vamos a instalar un servidor ftp externo como hicimos con `filezilla` en las practicas anteriores, sino que utilizaremos un servidor ftp que viene preinstalado como complemento adicional de `IIS`.

## 1. Instalar y acceder al servicio `FTP`

Para comenzar la practica lo primero que debemos hacer es instalar el servicio en nuestra MV, para ello nos dirigiremos al `administrador del servidor` y daremos clic en `agregar roles y servicios`.

![Imagen](img/001.png)

Dentro de este menú debemos buscar el `servidor web (IIS)` y dentro del mismo habilitar el subrol `servidor FTP`.

![Imagen](img/002.png)

Ahora solo debemos continuar la instalación por defecto y darle clic en `instalar`.

![Imagen](img/003.png)

Una vez hemos hecho esto al entrar al `Administrador de IIS` nos podemos encontrar múltiples opciones que nos permitirán gestionar nuestros sitios FTP.

![Imagen](img/004.png)

## 2. Creación de sitios `FTP`

En la actividad vamos a crear tres sitio distintos FTP, para agregar un nuevo sitio solo tenemos que hacer clic derecho en la pestaña sitios y pulsar en `Agregar sitio FTP...`.

![Imagen](img/005.png)

### 2.1. Primer sitio FTP - `C:`

El primer sitio que vamos a crear esta asociado a la unidad `C:` completa, este sitio no podrá ser accesible por usuarios anónimos, no hará uso de certificados `SSL` y solo podrá acceder el usuario `Administrador` de nuestro servidor con permisos de lectura y escritura.

![Imagen](img/006.png)

![Imagen](img/007.png)

![Imagen](img/008.png)

> En esta última captura cabe destacar que debemos seleccionar `autenticación básica`, de otra forma no nos será permitido acceder con el usuario `administrador` más adelante. Si se nos olvida siempre podemos modificarlo desde `autenticación FTP` en los menús que nos encontraremos al finalizar el sitio.
>
> ![Imagen](img/009.png)

Podemos comprobar que nuestro sitio esta funcionando accediendo mediante el navegador con la url `ftp://172.18.21.91`, que es la IP de mi servidor.

![Imagen](img/009-1.png)

![Imagen](img/010.png)

Cuando terminamos de crear nuestro sitio al acceder mediante `IIS` nos encontraremos con una multitud de opciones, veamos lo que hace cada una.

![Imagen](img/010-2.png)

1. `Aislamiento de usuario FTP`

    - Esta opción nos permite aislar a los usuarios de manera que no puedan acceder a determinadas carpetas que se podrían encontrar en su sesión de otra manera, como pueden ser las carpetas personales de otros usuarios FTP.

      ![Imagen](img/011.png)

1. `Autenticación FTP`

    - Esta opción ya la mencionamos en una anotación anteriormente, con ella podemos modificar el tipo de autenticación necesario para acceder a nuestro sitio FTP.

      ![Imagen](img/012.png)

1. `Compatibilidad con el firewall de FTP`

    - Como dice la descripción que encontramos dentro de esta opción, la misma nos permite configurar nuestro sitio FTP para aceptar conexiones pasivas de un firewall externo.

      ![Imagen](img/013.png)

1. `Configuración SSL de FTP`

    - Como nos indica el titulo en esta opción podemos modificar que nuestro servidor requiera o permita las conexiones SSL seleccionando el certificado necesario en cada caso.

      ![Imagen](img/014.png)

1. `Examen de directorios FTP`

    - En esta opción podemos modificar parámetros de como se verá nuestro sitio FTP al acceder desde un navegador, así como la opción de añadir información adicional a cada archivo o directorio que se encuentre en él.

      ![Imagen](img/015.png)

1. `Filtrado de solicitudes de FTP`

    - Desde esta opción se nos permite filtrar el contenido que será visible cuando se acceda al ftp, como podrían ser denegar la muestra de determinados archivos según su extensión, o la creación de segmentos ocultos.

      ![Imagen](img/016.png)

1. `Mensajes de FTP`

    - Aquí podemos configurar diferentes mensajes que se le mostrarán a los usuarios del sitio ftp, como puede ser un mensaje de bienvenida o de salida.

      ![Imagen](img/017.png)

1. `Registro FTP`

    - En este apartado se podrá configurar el sitio donde se guardan los registros de log del sitio FTP, así como la periodicidad de los mismos entre otras opciones.

      ![Imagen](img/018.png)

1. `Reglas de autorización de FTP`

    - Con esta opción podemos visualizar los usuarios que tienen acceso a nuestro sitio FTP y los permisos de los mismos. A su vez también podemos añadir nuevos usuarios permitidos o reglas de denegación.

      ![Imagen](img/019.png)

1. `Restricciones de direcciones IP y dominios de FTP`

    - Como su nombre indica, con esta opción podemos limitar que ciertos host puedan acceder o no a nuestro sitio FTP especificando su IP o el dominio al que pertenecen.

      ![Imagen](img/020.png)

1. `Sesiones actuales de FTP`

    - En este apartado podemos ver a los usuarios que están conectados a nuestro sitio, y si es preciso acabar con su conexión.

      ![Imagen](img/022.png)

      > Se puede comprobar debido a que podemos ver la sesión iniciada por de cuando probamos el acceso desde el navegador.
      >
      >![Imagen](img/023.png)

Sabiendo ahora como funcionan nuestros sitios FTP podemos comprobar que aparte de con el navegador también se puede acceder desde el explorador de archivos.

![Imagen](img/024.png)

![Imagen](img/025.png)

Vemos que nos ha solicitado la contraseña, para entrar ha sido necesario el usuario administrador de nuestro servidor, tal y como especificamos en la creación del sitio. Podemos comprobar también que disponemos de los permisos de lectura y escritura.

- Lectura:

  ![Imagen](img/026.png)

- Escritura:

  ![Imagen](img/027.png)

  ![Imagen](img/028.png)

#### 2.1.1 Acceder desde un cliente

También tenemos que comprobar que nuestro sitio FTP es accesible desde un cliente, en mi caso utilizaré una MV con Windows 7.

- Navegador:

  ![Imagen](img/030.png)

  ![Imagen](img/031.png)

- Explorador de archivos:

  ![Imagen](img/032.png)

  ![Imagen](img/033.png)

  ![Imagen](img/034.png)
  - Si accedemos a desde el administrador de nuestro sitio FTP a `sesiones actuales de FTP` podemos encontrarnos con la IP de nuestra máquina cliente.

    ![Imagen](img/035.png)

  - Pruebas de lectura:

    ![Imagen](img/036.png)

  - Pruebas de escritura:

    ![Imagen](img/037.png)

    ![Imagen](img/038.png)

Otra manera de acceder a nuestro sitio FTP es a través de un cliente FTP, en nuestro caso vamos a utilzar `WinSCP`, una herramienta que nos permite el acceso a sitios FTP, SFTP o hacer uso de SCP, un protocolo de copiado seguro.

Para instalar `WinSCP` accederemos a su página oficial https://winscp.net

![Imagen](img/039.png)

Descargamos el programa y lo instalamos con las opciones por defecto.

![Imagen](img/040.png)

![Imagen](img/041.png)

Ahora que ya tenemos instalado `WinSCP` iniciaremos el programa y configuraremos una conexión a nuestro sitio FTP, que en este caso utiliza el protocolo `FTP` sin certificado, la `ip`  de nuestro servidor, el puerto `21` y el usuario `administrador` con su correspondiente contraseña.

![Imagen](img/042.png)

![Imagen](img/043.png)

Al darle clic a conectar podemos ver los archivos de la ruta `C:` de nuestro servidor. También podemos comprobar que seguimos teniendo permisos de escritura.

![Imagen](img/044.png)

- Lectura:

  ![Imagen](img/045.png)

- Escritura:

  ![Imagen](img/046.png)

  ![Imagen](img/047.png)

  ![Imagen](img/048.png)

### 2.2. Segundo sitio FTP - `C:\inetpub\wwwroot`

Para la creación de nuestro segundo sitio FTP hemos seleccionado la ruta `C:\inetpub\wwwroot`, que es donde se encuentra la página por defecto de nuestro servidor web. En este sitio permitiremos el acceso a todos los usuarios de `active directory` con permisos de lectura y escritura. No permitiremos nuevamente el acceso a usuarios anónimos, y en esta ocasión "permitiremos" acceder mediante SSL con el certificado `autofirmado` que disponemos de la practica anterior.

- Primero creamos el sitio y le establecemos la ruta física.

    ![Imagen](img/049.png)

- Lo siguiente que debemos hacer es indicar la IP y el puerto a utilizar, en mi caso para no tener que desactivar el sitio anterior decidí cambiar de puerto al `2121`. En este apartado también es donde seleccionaremos la opción de que se permita `SSL` así como el certificado.

    ![Imagen](img/051.png)

    >En este apartado encontré varios errores por el hecho de seleccionar `habilitar nombres de host virtuales`, por lo cual más adelante decidí suprimir esta opción.
    >
    >  ![Imagen](img/054.png)

- Por último seleccionamos permisos para todos los usuarios, la autenticación básica y los permisos de lectura y escritura para todos ellos.

  ![Imagen](img/052.png)

#### 2.2.1. Comprobaciones de funcionamiento del sitio 2

 Ahora vamos a comprobar que funciona correctamente accediendo primero desde el servidor.

 - Navegador:

    - Probamos a entrar con el usuario `usu1`, perteneciente al `active directory`.

      ![Imagen](img/055.png)

      ![Imagen](img/056.png)

    - Podemos comprobar que efectivamente se ha iniciado sesión con el usuario `usu1` si lo buscamos en `sesiones actuales de FTP`

      ![Imagen](img/059.png)

- Explorador de archivos:

    - Probamos a entrar esta vez con el usuario `usu2`.

        ![Imagen](img/060.png)

        ![Imagen](img/061.png)

Ahora vamos a comprobar que podemos acceder desde el cliente, en este caso pasaremos directamente a probar la conexión desde `WinSCP`, en la que estableceremos una conexión `SSL` haciendo uso del certificado.

![Imagen](img/062.png)

> **Nota:** Al probar directamente el protocolo `SFTP` la conexión no se realizaba correctamente, por eso decidí utilizar el protocolo `FTP` con la clave de cifrado `SSL`.

![Imagen](img/063.png)

Probemos a intentar copiar un archivo.

![Imagen](img/064.png)

![Imagen](img/065.png)

Con esto comprobamos que los permisos de escritura funcionan correctamente.

> **Nota:** Desde `WinSCP` podemos comprobar fácilmente que al hacer usos de puertos diferentes podemos utilizar ambos sitios FTP al mismo tiempo.
>
>![Imagen](img/066.png)
>
>![Imagen](img/067.png)

### 2.3. Tercer sitio FTP - `C:\FTP3`

En el tercer sitio FTP debemos asociar la carpeta que queramos, en mi caso he creado el directorio `C:\FTP3` y un conjunto de subcarpetas/archivos dentro. En este sitio web permitiremos el acceso anónimo y solo se podrá consultar y leer.

- Creamos el sitio FTP indicándole su ruta.

  ![Imagen](img/068.png)

- Seleccionamos un nuevo puerto desde donde poder acceder, en mi caso `2122`.

  ![Imagen](img/069.png)

- Para finalizar permitimos el acceso a usuarios anónimos de solo lectura, y seleccionamos la autenticación anónima.

  ![Imagen](img/070.png)

#### 2.3.1. Comprobaciones de funcionamiento del sitio 3

Comprobemos que funciona primero desde servidor.

- Navegador:

  ![Imagen](img/071.png)

  > Desde el primer momento nos percatamos de que se accede directamente, sin necesidad de contraseñas ni usuarios.

  ![Imagen](img/072.png)

- Explorador de archivos:

  ![Imagen](img/073.png)

  ![Imagen](img/074.png)

  Podemos comprobar a la hora de pasar un archivo que efectivamente el usuario anónimo no tiene permisos de escritura.

Si probamos el acceso desde `WinSCP` con el cliente marcando el usuario anónimo vemos que nos permite acceder.

![Imagen](img/074-1.png)

![Imagen](img/075.png)

![Imagen](img/076.png)

![Imagen](img/077.png)

A la hora de intentar copiar un archivo vemos nuevamente que no disponemos de ese permiso.

## 3. Agregar sitios FTP al servidor DNS

Para finalizar con Windows vamos a crear un nuevo registro `A` en nuestro servidor DNS para que el servidor y el cliente puedan acceder a los sitios FTP usando la URL `ftp.miempresa.com`.

![Imagen](img/078.png)

Simplemente añadiendo este registro ya seremos capaces de acceder a todos los sitios FTP desde la URL.

### 3.1. Comprobaciones

#### 3.1.1. Sitio FTP 1 - `C:`

- Servidor:

  ![Imagen](img/079.png)

  ![Imagen](img/080.png)

- Cliente:

  ![Imagen](img/081.png)

  ![Imagen](img/082.png)

#### 3.1.2. Sitio FTP 2 - `C:\inetpub\wwwroot`

Para acceder utilizando la URL a los demás sitios será preciso indicarle el puerto correspondiente.

- Servidor:

  ![Imagen](img/083.png)

  ![Imagen](img/084.png)

- Cliente:

  ![Imagen](img/085.png)

  ![Imagen](img/086.png)

  > Este mensaje no lo mostré antes, pero a la hora de acceder al FTP 2 con el certificado `autofirmado` nos indica como que no es seguro por no estar firmado por ninguna entidad reguladora, esto lo sabíamos de practicas anteriores.

  ![Imagen](img/087.png)

#### 3.1.3. Sitio FTP 3 - `C:\FTP3`

- Servidor:

  ![Imagen](img/088.png)

- Cliente:

  ![Imagen](img/089.png)

  ![Imagen](img/090.png)

Hecho esto podemos dar por finalizada la parte de la practica relacionada con FTP Windows.
