# U4-A2-B Práctica de IIS Windows 2012 Server

## Instalación y Configuración de un Servidor Web Avanzado Internet Information Server – Carpetas Privadas

En esta práctica vamos a generar un numero de carpetas privadas dentro de nuestra web, con las que solo podrán acceder determinados usuarios de nuestro servidor.

Para ello vamos a crear un nuevo sitio web `empleados.miEmpresa.com` destinado a almacenar información privada de los empleados.

## 1. Crear la dirección fisica de la web

Antes de comenzar vamos a diseñar las rutas donde se situara nuestra página web, esta la localizaremos en:

- `C:\miEmpresa\empleados`:

Dentro de esta vamos a crear un conjunto de carpetas para los empleados y una global para todas estas que llamaremos `"comun"`.

![Imagen](img/01.png)

Y como siempre antes de crear nuestra página web vamos a añadir a nuestro servidor `DNS` un registro A.

![Imagen](img/02.png)

## 2. Crear sitio web

Ahora vamos a crear el sitio web, para ello nos dirigimos a al administrador de `IIS` y añadimos `empleados.miempresa.com`.

![Imagen](img/03.png)

![Imagen](img/04.png)

Para probar nuestro sitio web vamos a añadir a cada uno de las subcarpetas un `index.html` y activaremos en la carpeta principal `C:\miEmpresa\empleados` el `examen de directorios` para que se vean las carpetas auto indexadas.

![Imagen](img/05.png)

![Imagen](img/06.png)

> **Nota:** Para ver rápidamente el contenido de las subcarpetas en Windows podemos desde un terminal ejecutar `"TREE <unidad><ruta> /F /A"`

![Imagen](img/07.png)

### 2.1. Comprobaciones iniciales

Vamos a probar que las páginas funcionan rápidamente.

- `empleados.miempresa.com`:

  ![Imagen](img/08.png)

- `empleados.miempresa.com/stan`:

  ![Imagen](img/12.png)

- `empleados.miempresa.com/kyle`:

  ![Imagen](img/11.png)

- `empleados.miempresa.com/cartman`:

  ![Imagen](img/09.png)

- `empleados.miempresa.com/comun`:

  ![Imagen](img/10.png)

## 3. Configurar acceso a las carpetas iniciales

Una vez comprobado que nuestras páginas funcionan correctamente debemos para cada una de las carpetas que queremos privatizar realizar las siguientes acciones:

      - Deshabilitar la `autenticación anónima` en cada subcarpeta.

      - Habilitar la `autenticación básica` en cada subcarpeta.

      - En la carpeta original permitir la `autenticación anónima`.

Para ello en cada apartado del sitio web debemos acceder a la opción `Autenticación`.

![Imagen](img/16.png)

- `empleados`:

  ![Imagen](img/13.png)

- `Cartman`:

  ![Imagen](img/14.png)

- `comun`:

  ![Imagen](img/15.png)

- `Kyle`:

  ![Imagen](img/17.png)

- `Stan`:

  ![Imagen](img/18.png)


## 4. Crear los usuarios y el grupo para autenticarse

Queremos que cada empleado pueda entrar en su carpeta personal de manera que esta le exija autenticarse con un usuario y contraseña, para ello debemos crear los usuarios `stan`, `kyle` y `cartman` como usuarios de `active directory`.

Para hacer esto nos dirigimos al `Administrador del servidor` y en herramientas seleccionamos `Usuarios y equipos de Active Directory`.

![Imagen](img/19.png)

Dentro del menú que nos ofrece la herramienta tenemos que hacer clic derecho en `users` y seleccionar `Nuevo` -> `Usuario`.

![Imagen](img/20.png)

Esto nos ejecutara un asistente que nos pedirá el nombre, la contraseña y otros parámetros para crear el usuario.

![Imagen](img/21.png)

![Imagen](img/22.png)

![Imagen](img/23.png)

Repetimos el proceso con el resto de los usuarios hasta que tengamos todos los que nos hacen falta.

![Imagen](img/24.png)

Así mismo crearemos un grupo llamado `empleados` que contendrá todos estos usuarios.

![Imagen](img/25.png)

![Imagen](img/26.png)

Con el grupo creado vamos a las propiedades del mismo y añadimos en la pestaña `Miembros` a nuestros tres usuarios.

![Imagen](img/27.png)

## 5. Configurar el acceso a la carpeta física del sitio web

Para especificar que usuario puede acceder a cual carpeta debemos especificarlo desde las propiedades de seguridad de cada carpeta, lo primero que haremos será deshabilitar los permisos heredables de nuestra carpeta principal `C:\miEmpresa\empleados`.

Para ello nos dirigimos a las opciones de seguridad avanzadas de la carpeta y seleccionamos `Deshabilitar herencia`.

![Imagen](img/28.png)

![Imagen](img/29.png)

![Imagen](img/30.png)

![Imagen](img/31.png)

Para los permisos de la mismas añadimos `Administradores` con control total y el grupo `empleados` con permisos de lectura y ejecución, mostrar el contenido de la carpeta y lectura.

![Imagen](img/32.png)

![Imagen](img/33.png)

> **Nota:** Es posible que con esta configuración nos de problemas el `examen de directorios` en `empleados.miempresa.com`, para solucionarlo debemos añadir también al usuario `IIS_IUSRS` dentro de los usuarios con privilegios para esta carpeta.

### 5.1. Configurar el acceso a las carpetas físicas de cada empleado

De la misma manera que hicimos con la carpeta principal debemos deshabilitar la herencia para cada una de las subcarpetas de los empleados y asignar como usuarios privilegiados lo siguiente.

- `C:\miEmpresa\empleados\Cartman`:

  - Administradores con permisos de control total.
  - Usuario `Cartman` con permisos los permisos que creamos apropiados.

  ![Imagen](img/34.png)


- `C:\miEmpresa\empleados\Kyle`:

  - Administradores con permisos de control total.
  - Usuario `Kyle` con permisos los permisos que creamos apropiados.

  ![Imagen](img/35.png)


- `C:\miEmpresa\empleados\Stan`:

  - Administradores con permisos de control total.
  - Usuario `Stan` con permisos los permisos que creamos apropiados.

  ![Imagen](img/36.png)

### 5.2. Configurar el acceso a la carpeta física `"comun"`

Al igual que con las anteriores deshabilitamos la herencia de esta carpeta. La diferencia con la demás será que no configuraremos ningún usuario privilegiado, sino que será el grupo `empleados`, ya que en esta carpeta buscamos que sea común para todos estos.

![Imagen](img/37.png)

## 6. Comprobaciones finales

Vamos a comprobar ahora que todos las configuraciones funcionan correctamente tanto desde el servidor como desde un cliente.

### 6.1. Servidor

Primero comprobemos que podemos acceder a `empleados.miempresa.com`.

![Imagen](img/38.png)

> En este punto es donde sabremos si debemos hacer uso de la anotación en el punto "2.".

Probemos que al acceder al empleado `Cartman` nos pide el usuario de este.

![Imagen](img/39.png)

![Imagen](img/40.png)

Una vez identificado nos deja acceder, al mismo tiempo podemos comprobar que acceder a `"comun"` también es posible sin tener que logearnos, ya que el usuario `cartman` pertenece al grupo empleados.

![Imagen](img/41.png)

Sin embargo si intentamos acceder al empleado `Stan` nos pedirá nuevamente que nos identifiquemos con otro usuario, haciéndolo comprobamos que también es accesible con su usuario.

![Imagen](img/42.png)

![Imagen](img/43.png)

![Imagen](img/44.png)

### 6.2. Cliente

Comprobemos que sucede lo mismo en un cliente con Windows 7.

- Acceder a la página principal.

  ![Imagen](img/45.png)

- Acceder al empleado `Kyle` haciendo "login" con su usuario.

  ![Imagen](img/46.png)

  ![Imagen](img/47.png)

- Acceder a `"comun"` mientras estamos logeados con el usuario `kyle`.

  ![Imagen](img/48.png)

- Acceder al empleado `Cartman` haciendo "login" con su usuario.

  ![Imagen](img/49.png)

  ![Imagen](img/50.png)

Una vez hechas todas esta comprobaciones podemos dar por acabada esta actividad.
