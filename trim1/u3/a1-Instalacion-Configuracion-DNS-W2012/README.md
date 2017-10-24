# Instalación y configuración DNS Windows 2012 server

En este actividad vamos a instalar y configurar un servidor DNS en una máquina con Windows 2012 Server. A continuación veremos los pasos y la prueba de funcionamiento de la misma.

## 1. Instalación del servicio DNS

En nuestro caso podemos saltarnos este paso porque al instalar `Active directory` en las máquinas se activa por defecto, en caso contrario deberemos añadir `servicios DNS` en `agregar roles`. Aun así estableceremos las IPs a nuestro servidor para que lo localicen desde los clientes en la práctica.

- Servidor:

![Imagen](img/01.png)

- Cliente:

![Imagen](img/02.png)

> El cliente no tiene porque tener una IP estática, pero decidí hacerlo así por que al momento de encender la máquina habían muchos compañeros compartiendo su ámbito DHCP.

## 2. Configuración del servicio DNS

Una vez instalado el servicio podemos configurarlo desde `administrador del servidor` -> `Herramientas` -> `DNS`

![Imagen](img/03.png)

### 2.1. Zona de búsqueda directa

Lo primero que haremos será crear una zona de búsqueda directa, con esta podremos configurar todas las IPs que queremos traducir con sus registros `A`, `CNAME`, `MX`... etc.

![Imagen](img/04.png)

Podemos dejar la configuración estándar

![Imagen](img/05.png)

![Imagen](img/06.png)

![Imagen](img/07.png)

> Este será el nombre de nuestra zona.

![Imagen](img/08.png)

### 2.2. Zona de búsqueda inversa

Vamos a crear una nueva zona, esta vez una zona inversa, la cual guardará la información del nombre que se le da a las direcciones IP con registros `PTR`

![Imagen](img/09.png)

Nuevamente podemos realizar una configuración estándar, indicando nuestra red al final.

![Imagen](img/10.png)

![Imagen](img/11.png)

![Imagen](img/12.png)

![Imagen](img/13.png)

## 3. Configurar reenviadores

Ahora vamos a configurar los reenviadores de nuestro servicio DNS. Estos reenviadores funcionan de manera que cuando nuestro servidor no encuentra las direcciones en sus zonas reenvía la petición a otros servidores DNS que le hayamos indicado.

![Imagen](img/14.png)

Dentro de está opción podemos añadirle nuestra puerta de enlace y/o un DNS público como `8.8.8.8`.

![Imagen](img/15.png)

### 3.1. Comprobar que funciona como DNS caché

Ahora que hemos configurado los reenviadores nuestro servidor debería poder comportarse como un DNS caché, por lo para comprobar que funcionan intentaremos acceder a sitios web en internet.

![Imagen](img/18.png)

![Imagen](img/19.png)

En las imágenes se puede comprobar con `nslookup` que el servidor DNS de la máquina es el de nuestro servidor, y que a través de él podemos acceder a sitios web que no se encuentran especificados en el mismo.

## 4. Configurar el servidor como DNS Maestro

Ahora vamos a configurar nuestro servidor como DNS maestro, para eso vamos a crear varios registros dentro nuestra zona de búsqueda directa. Estos registros serán:

1. Un alias para nuestro servidor denominado `server`.

![Imagen](img/.png)

2. Una impresora con IP fija denominada `printer`, sin necesidad de alias.

![Imagen](img/.png)

3. Un servidor de correo denominado `correo`, asociado a una dirección en nuestro servidor.

![Imagen](img/.png)

Por último vamos a crear una subzona denominada `servicios` en el cual agregaremos:

![Imagen](img/.png)

1. Un servidor ftp (asociado a la misma IP de nuestro servidor).

![Imagen](img/.png)

2. Una impresora nueva (Con una IP fija).

![Imagen](img/.png)

3. El equipo del administrador del sistema (Con una IP fija).

![Imagen](img/.png)
