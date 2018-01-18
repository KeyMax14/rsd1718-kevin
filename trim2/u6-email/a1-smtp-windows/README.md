# U6_A1 - Servicio SMTP Windows 2012 Server

En esta práctica vamos a configurar el servicio SMTP en una máquina con Windows 2012 Server.

## 1. Instalación Servicio SMTP en Windows 2012 Server

Para instalar el servicio nos dirigiremos al `administrador del servidor` -> `Agregar roles`.

![Imagen](img/001.png)

Desde aquí, en el caso de tener ya instalado el servidor `ISS`, tendremos que avanzar hasta el apartado `características`, donde buscaremos y activaremos la opción `Servidor SMTP`.

![Imagen](img/002.png)

![Imagen](img/003.png)

Una vez instalado podemos configurar el servicio si accedemos a `herramientas` -> `Administrador de IIS 6.0`.

![Imagen](img/004.png)

![Imagen](img/005.png)

## 2. Configurando Servicio SMTP

Ahora desde el administrador de `IIS 6.0` vamos a configurar algunos parámetros de nuestro servidor. Hacemos clic derecho en `[SMTP virtual server]` -> `propiedades`.

![Imagen](img/006.png)

- Asignamos la dirección IP a todas las no asignadas.
- Limitamos el número de conexiones a '50'.

  ![Imagen](img/007.png)

- Habilitamos el registro en formato `W3C`, que se ejecute de forma diaria y lo asignamos a la carpeta `C:\system32\LogFiles`

  ![Imagen](img/008.png)

  ![Imagen](img/009.png)

- Configuramos el envío de mensajes desde nuestra red local y limitamos la conexión a una lista de IPs.

  ![Imagen](img/010.png)

  ![Imagen](img/011.png)

- Permitimos la autenticación anonima.

  ![Imagen](img/012.png)

  ![Imagen](img/013.png)

Reiniciamos el servicio para cargar todos los cambios. Una vez hecho esto vamos a comprobar que existe el dominio del `Active Directory` de manera predeterminada.

![Imagen](img/014.png)

![Imagen](img/015.png)

Comprobado esto vamos a crear un nuevo dominio tipo alias para disponer de cuentas en otro dominio, por ejemplo `miempresa.com`.

![Imagen](img/016.png)

![Imagen](img/017.png)

![Imagen](img/018.png)

![Imagen](img/019.png)

Para finalizar vamos a comprobar también que se han creado las carpetas necesarias para el servidor email en la ruta `C:\inetpub\mailroot`.


![Imagen](img/020.png)

# 3. Configuración del cliente
