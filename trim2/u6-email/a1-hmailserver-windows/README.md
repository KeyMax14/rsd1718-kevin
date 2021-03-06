# Servidor de Correo hMailServer

![Imagen](img/000.png)

En esta actividad vamos a configurar el servidor de correo `hMailServer` en nuestra máquina Windows Server 2012. A diferencia de la herramienta que teníamos en IIS esta vez podremos configurar nuestro servidor para correo entrante y saliente.

## 1. Desinstalar el Servidor SMTP previo

Antes de instalar el servidor de correo `hMailServer` tendremos que desinstalar el servidor de correo de Windows 2012 Server. Para hacer esto nos dirigimos desde `Administrador del servidor` a la opción `Quitar roles o características`.

- Quitamos la característica `Servidor SMTP`.

  ![Imagen](img/001.png)

  ![Imagen](img/002.png)

## 2. Descargamos e instalamos `hMailServer`

Vamos a descargar en nuestro servidor la última versión estable de `hMailServer`, para ello nos dirigimos a su página oficial https://www.hmailserver.com/download

  ![Imagen](img/003.png)

Una vez descargado vamos a instalar el programa con sus opciones por defecto.

![Imagen](img/004.png)

![Imagen](img/006.png)

- En el siguiente paso podemos elegir la base de datos que vamos utilizar, en nuestro caso utilizaremos una base de datos propia generada por el programa.

  ![Imagen](img/007.png)

- Establecemos la contraseña.

  ![Imagen](img/008.png)

> En el caso de encontrarnos lo siguiente:
>
> ![Imagen](img/009.png)
>
> Tendremos que instalar `.NET framework 3.5` desde `agregar roles`
>
>![Imagen](img/010.png)
>
>![Imagen](img/011.png)

Después de esto ya podremos acceder al servidor.

![Imagen](img/012.png)

> **Posible fallo:** Es posible que a la hora de conectarte te muestre un mensaje de que no se puede conectar con la base de datos, en este caso desinstalamos el programa y eliminamos el directorio `C:\Program Files (x86)\hMailServer` y volvemos a reinstalarlo. Los fallos pueden venir a causa de que no teníamos instalado previamente el `.NET framework 3.5`.
>
> ![Imagen](img/018.png)
>
> ![Imagen](img/019.png)

## 3. Creamos nuevos Dominios de Correo

Ahora que ya tenemos instalado `hMailServer` vamos al apartado `Domains` y añadimos un par de dominios nuevos.

![Imagen](img/016.png)

![Imagen](img/020.png)

- Creamos los dominios `asir.edu` y `srd.edu`.

  ![Imagen](img/021.png)

  ![Imagen](img/022.png)

## 4. Realizar diagnósticos sobre los dominios

Desde el propio administrador de hMailServer podemos comprobar si nuestros dominios están bien configurados, para ello nos dirigimos a `utilities` -> `Diagnostics`.

- Seleccionamos un dominio, como puede ser `asir.edu` y pulsamos `Start`.

  ![Imagen](img/023.png)

  ![Imagen](img/024.png)

Encontramos varios errores relacionados con los registros MX que no se encuentran o que no esta especificado el directorio para hacer backups, el primero lo solucionaremos más tarde configurando el DNS pero para arreglar el primero solo debemos dirigirnos a `utilities` -> `Backup`.

- Desde este menú seleccionamos la ruta donde se guardan las copias y seleccionamos que se guarden las opciones, los dominios y los **mensajes**.

  ![Imagen](img/025.png)

Una vez hecho esto si hacemos un diagnostico en ambos vemos que ya no nos muestra un error.

![Imagen](img/026.png)

![Imagen](img/027.png)

## 5. Creación de usuarios

Si queremos crear usuarios en nuestros dominios solo tendremos que ir al dominio que creamos y en el apartado `accounts` clicamos en `add user..`. Vamos a crear dos usuarios en cada uno de los dominios.

- Dominio `asir.edu`:

  - Usuario `kyle@asir.edu`:

    ![Imagen](img/028.png)

    Para el usuario `kyle` añadimos la opción `Auto-reply`, que contesta directamente a los mensajes recibidos.

    ![Imagen](img/029.png)

    También le añadimos la opción `signature`, en lo que añadimos una firma al final del mensaje.

    ![Imagen](img/030.png)

    ![Imagen](img/031.png)

  - Usuario `eric@asir.edu`:

    ![Imagen](img/032.png)

    Para este usuario habilitamos la opción `Forwarding`, que lo que hace es redireccionar los mensajes al correo que especificamos.

    ![Imagen](img/033.png)

    ![Imagen](img/034.png)

- Dominio `srd.edu`:

  - Creamos los usuario `kenny` y `stan` con diferentes opciones.

    ![Imagen](img/035.png)

## 6. Configuramos el servicio DNS

Vamos a configurar el servicio DNS para poder acceder desde los clientes de una forma sencilla. Para ello vamos a crear dos zonas de búsqueda directa (`asir.edu` y `srd.edu`).

![Imagen](img/036.png)

![Imagen](img/039.png)

![Imagen](img/040.png)

Como vemos en las capturas añadimos los registros tipo `A` y registros tipo `MX` para cada uno de ellos.

Si ahora volvemos al servidor al apartado de `Diagnostics` podemos ver que el dominio `srd.edu` ya esta corregido el error.

![Imagen](img/041.png)

Por algún motivo seguramente referido a la poca fiabilidad de la propagación  `DNS` el dominio `asir.edu` sigue mostrando un error respecto a los registros `MX`, pero con un `nslookup` podemos comprobar que funciona correctamente.

![Imagen](img/043.png)

![Imagen](img/042.png)

> Esperando lo suficiente finalmente se muestra todo correcto en los diagnosticos.
>
> ![Imagen](img/044.png)

## 7. Diferentes configuraciones de hMailServer

Para finalizar con las configuraciones de hMailServer vamos a añadir varias opciones:

- Opción en el protocolo SMTP - Nombre de host:

  ![Imagen](img/045.png)

- Configuración de listas negras:

  ![Imagen](img/046.png)

- Rango de IPs permitidas (Todas):

  ![Imagen](img/047.png)

- `Auto-Ban`:

  Esta opción es prioritaria seleccionarla se queremos poder recibir el correo entrante.

  ![Imagen](img/048.png)

- `Mirror`:

  En esta opción podemos duplicar la información recibida el servidor de email para balancear cargar o tener un servicio ininterrumpido.

  ![Imagen](img/049.png)

- `Loggin`:

  En esta opción seleccionamos los archivos de log que queremos guardar.

  ![Imagen](img/050.png)

## 8. Configurar cliente

En este apartado vamos a reutiliza el cliente `Opera Mail` utilizado anteriormente, borramos los mail que habíamos creado y añadimos a todos nuestros nuevos usuarios.

- Usuario `kyle@asir.edu`:

  ![Imagen](img/051.png)

  ![Imagen](img/052.png)

  > **Nota:** Es importante que el nombre de usuario sea la dirección completa para que funcione correctamente.

  ![Imagen](img/053.png)

  > Aquí usamos el nombre DNS que creamos con anterioridad.

- Usuario `eric@asir.edu`:

  Creamos el usuario igual que el anterior cambiando al usuario.

  ![Imagen](img/055.png)

- Usuario `kenny@srd.edu`:

  ![Imagen](img/056.png)

  ![Imagen](img/057.png)

  ![Imagen](img/058.png)

- Usuario `stan@srd.edu`:

  Igual que `kenny` pero cambiando el nombre.

  ![Imagen](img/059.png)

## 9. Pruebas de envío desde los clientes

Vamos a probar como se envían y se reciben los mensajes desde todos los clientes.

- `eric@asir.edu` -> `kyle@srd.edu`:

  ![Imagen](img/060.png)

  ![Imagen](img/061.png)

  El correo se ha recibido correctamente.

- `kenny@srd.edu` -> `eric@asir.edu`:

  ![Imagen](img/062.png)

  En este caso como se menciona en el mensaje, el usuario `eric` tiene habilitado el `Forwarding` hacia el usuario `kyle`. A su vez el usuario `Kyle` tiene un `Auto-reply`, por lo que se enviará el mensaje original a `eric` y `kyle`, mientras que la auto-respuesta de `kyle` se enviará a `kenny` y a `eric`.

  - `eric`:

    ![Imagen](img/063.png)

    ![Imagen](img/064.png)

    ![Imagen](img/065.png)

  - `kyle`:

    ![Imagen](img/066.png)

  - `kenny`:

    ![Imagen](img/067.png)

- `kyle@asir.edu` -> `stan@srd.edu`:

  ![Imagen](img/068.png)

  En este caso como dice el mensaje, el usuario `kyle` tiene establecida una firma al final de sus mensajes, así que aunque no este escrito enviará también el texto `un saludo`.

  ![Imagen](img/069.png)

## 10. Crear una lista de distribución

El servidor `hMailServer` también tiene la característica de agrupar mail en una misma dirección utilizando grupos llamados `listas de distribución`. Vamos a crear una lista de distribución para los usuarios `kenny@srd.edu` y `stan@srd.edu`.

Para hacer esto nos dirigimos a `Domains` -> `srd.edu` -> `Distribution list`, asignamos un nombre a nuestra lista y pulsamos en `save`.

![Imagen](img/070.png)

Una vez realizado esto nos desplazamos a la pestaña `Members` y añadimos ambos usuarios.

![Imagen](img/071.png)

### 10.1. Comprobar el funcionamiento de la lista

Para comprobar que la lista vamos a enviar un correo a `ParaTodo@srd.edu`.

![Imagen](img/072.png)

Al enviar el correo vemos que recibimos dos correos nuevos en los usuarios `kenny@srd.edu` y `stan@srd.edu`.

![Imagen](img/073.png)

- `kenny`:

  ![Imagen](img/074.png)

- `stan`:

  ![Imagen](img/075.png)

Una vez realizado esto podemos dar por finalizada la práctica.
