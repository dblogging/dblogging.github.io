---
layout: post
title: "MySQL - ZIP"
subtitle : "Instalación y configuración"
date: 2022-02-01 23:25:13 -0400
background: "/img/posts/mysql-install/bg.jpg"
categories: "sql"
---


## Contenido

1. [Descarga y extrae los archivos.](#download)
1. [Archivos de opciones](#options_file)
1. [Primera conexión](#first_connect)
1. [Establecer contraseña](#change_password)
1. [Configurar MySQL como un servicio](#install_as_service)




### Descarga de los archivos {#download} 

Descargamos el archivo zip &#x25b6; [aquí](https://dev.mysql.com/downloads/file/?id=509736){:target="_blank"}

>Ya esta seleccionado el archivo.

![image 01](/img/posts/mysql-install/01.png)

> Solo damos clic en donde está señalado en la siguiente ilustración y comenzará la descarga.

![image 02](/img/posts/mysql-install/02.png)

Extraemos el contenido del archivo zip en el directorio deseado.  

>En esta guía lo vamos a descomprimir en la raíz del disco C y todos los archivos estarán dentro de la carpeta que nombraremos MySQL8

![image 03](/img/posts/mysql-install/03.png)



[![back](https://img.shields.io/badge/regresar%20contenido%20principal-%E2%86%A9-blue?style=for-the-badge&logo=readthedocs&logoColor=%23FAC173)](#contenido)


### Archivo de opciones {#options_file}

Para especificarle opciones al servidor durante su inicio podemos hacerlo desde la línea de comandos pasandole como argumento las diferentes opciones que puedes encontrar en la [documentación oficial](https://dev.mysql.com/doc/refman/8.0/en/server-options.html) o bien colocando las opciones en un **archivo de opciones** (lo recomendado). Aquellas opciones se usarán cada vez que se inicie el servidor, esto es especialmente necesario en las siguiente circuntancias:

- El directorio de instalación y de datos son diferentes a los predeterminado. 
- Es necesario afinar la configuración del servidor.  

Un archivo de opciones se utiliza para que MySQL lea los parámetros declarados dentro del archivo de opciones y pueda iniciar el servidor de base de datos con las configuraciones que deseamos.  

El archivo dentro tiene una sección exclusiva para el las opciones del servidor **[mysqld]** donde podremos definir la siguiente párametros que nos interesa: 

- **basedir**: el directorio base de instalación. (En nuestro caso el directorio base de ejemplo es  **C:\MySQL8**)  

- **datadir**: la ubicación del directorio de datos. (Este se crea por defecto dentro del directorio base de instalación cuando inicializamos por primera vez el servidor, por ende es importante crear un archivo de opciones previamente para establecer donde queremos que se cree este directorio de datos).  

### &#x1f4c3; Crear un nuevo archivo de opciones


Creaamos un nuevo archivo dentro del directorio base de la instalación y lo vamos a nombrar **my.ini**

![image 04](/img/posts/mysql-install/04.png)

Este archivo puede modificarse con cualquier editor de texto, como el bloc de notas.  

![image 05](/img/posts/mysql-install/05.png)

En la sección **[mysqld]** asignaremos los valores que tendrán los parámetros `basedir` y `datadir` que serán las rutas de destino en nuestro sistema.

```
[mysqld]
basedir=<PATH>
datadir=<PATH>
```

![image 06](/img/posts/mysql-install/06.png)


>Si quieres ver más sobre las opciones para [mysqld] ve a la [documentación oficial](https://dev.mysql.com/doc/refman/8.0/en/option-files.html){:target="_blank"}

**Aquí están algunas opciones que tengo definidas en el archivo:** 

```mysql.ini
[client]
port=3306
socket=/temp/mysql.shock

[mysqld]
basedir=C:/MySQL8
datadir=C:/data
port=3306
socket=/temp/mysql.shock
key_buffer_size=16M
max_allowed_packet=8M

[mysqldump]
quick
```

>Sin embargo no es relevante, porque lo importante son **datadir** y **basedir**, el puerto y las demás opciones vienen por defecto.  
Si la carpeta que hemos definidos para los datos no existe, la creará por nosotros, pero si debe existir el destino de ruta que hemos indicado.

Hecho lo anterior para inicializar una instalación de MySQL, en caso de que no hayamos creando el archivo de opciones, se creara un directorio **data** dentro del directorio de instalación y dentro de ese directorio se crearan las bases de datos del sistema llenando las tablas del sistema mysql.

Para iniciar la instalación de MySQL nos situamos dentro del directorio raíz y abrimos una consola. Luego ejecutamos el siguiente comando:

```sh
mysqld --initialize o --initialize-insecure.
```

![image 07](/img/posts/mysql-install/07.png)

**Este comando hace lo siguiente:** 

- Incializa el directorio de datos de MySQL y crea las tablas del sistema.
- Instala el esquema [sys](https://dev.mysql.com/doc/refman/8.0/en/sys-schema.html)
- Crea una cuenta administrativa. 
- Se crea una sola cuenta administrativa **'root'@'localhost'** con una contraseña generada aleatoriamente, que se marca como caducada.
- No se crean cuentas de usuario anónimo.
- No se crea ninguna base de datos como **test** accesible para todos los usuarios.

**Ahora podemos iniciar el servidor con el comando:**  

```sh
mysqld --console
```

![image 08](/img/posts/mysql-install/08.png)

Al permitir el acceso del firewall del sistema, nos debe mostrar el siguiente mensaje al ejecutarse.

![image 09](/img/posts/mysql-install/09.png)


La opción `--console` es para ver el log en la línea de comandos y nos debe indicar que se encuentra listo para recibir conexiones entrantes.

En caso que el destino para las bases de datos es otro, debe copiar todo el contenido del directorio data en la nueva ubicación. Por ejemplo, si desea utilizarlo **C:\mydata** como directorio de datos, puede hacer dos cosas:

1. Mueva todo el directorio data y todo su contenido desde la ubicación predeterminada. Por ejemplo **C:\Path\installation\data** a **C:\mydata** y luego detiene el servidor, realiza los cambios en el archivo de opciones y se vuelve a iniciar el servidor.

2. Utilice una opción `--datadir` para especificar la nueva ubicación del directorio de datos cada vez que inicie el servidor. Ej:

```mysql
mysqld --datadir 'path\your\data'
```

![image 10](/img/posts/mysql-install/10.png)


[![back](https://img.shields.io/badge/regresar%20contenido%20principal-%E2%86%A9-blue?style=for-the-badge&logo=readthedocs&logoColor=%23FAC173)](#contenido)


### Primera conexión {#first-connect}

Independiente de la configuración que hemos dado para conectarnos al servidor, primero que nada debemos inicializarlo, y luego conectarnos como clientes, dentro de la carpeta data se encuentra un archivo llamado con el nombre de tu equipo y la extensión `.err` abrimos ese archivo con el bloc de nota y buscamos el password generado para el usuario root de manera temporal:

![image 11](/img/posts/mysql-install/11.png)

> Copiamos el password aleatorio que se generó

![image 12](/img/posts/mysql-install/12.png)

> Nos movemos a la carpeta bin, podemos hacerlo abriendo una consola y usar el comando: 

```sh
cd bin
```

LLamamos al programa **mysql** y le pasamos los argumento para conectarnos

> Pegamos el password aleatoriao cuando nos solicite.

![image 13](/img/posts/mysql-install/13.png)


Ahora la primera tarea antes de comenzar a manipular bases de datos, será cambiar el password de nuestro usuario root. De hecho si intentamos ejecutar un comando sql nos va a requerir esta acción.

![image 14](/img/posts/mysql-install/14.png)


[![back](https://img.shields.io/badge/regresar%20contenido%20principal-%E2%86%A9-blue?style=for-the-badge&logo=readthedocs&logoColor=%23FAC173)](#contenido)

---


### Esteblecer una nueva contraseña {#change_password}


El comando **`ALTER USER`** modifica las cuentas de MySQL. Permite modificar las propiedades de autenticación, SSL/TLS, límite de recursos y administración de contraseñas para las cuentas existentes. 

Para cada cuenta afectada, **`ALTER USER`** modifica la fila correspondiente en la tabla `mysql.user` del sistema.

**Sintaxis, para cambiar el password para el usuario conectado** 

```sql
ALTER USER USER() IDENTIFIED BY 'proPassword123';
```

La función `USER()` hace referencia a su propia contraseña sin tener que nombrarla literalmente (ejemplo; root@localhost). A continuación hemos dado un password al usuario root con el que nos conectamos con la contraseña generada temporalmente por el servidor al momento de su instalación.

![image 15](/img/posts/mysql-install/15.png)

Ahora recargamos la tabla de privilegios con el siguiente comando: 

```sql
FLUSH PRIVILEGES;
```

Ahora ya podemos conectarnos con la nueva contraseña que hemos asignado, podemos consultar en la tabla de usuario y el complemento de autenticación con el siguiente comando: 

```sql
SELECT user, plugin FROM mysql.user;
```

![image 16](/img/posts/mysql-install/16.png)


>Adicionalmente, podemos establecer esta ruta de donde lanzamos el servidor como variable de entorno para que nos resulte más comodo lanzar el servidor desde cualquier ubicación. Con el siguiente comando podemos establecer la ruta de instalación de MySQL a la variable **path** del usuario:

**En CMD**:

```bat
SETX PATH "%path%;"C:\MySQL8\bin\
```

>Cerramos la ventana de CMD y abrimos una nueva  

**Para invocar a mysql en CMD o PowerShell**:

```bat
mysql -u root -p
```


[![back](https://img.shields.io/badge/regresar%20contenido%20principal-%E2%86%A9-blue?style=for-the-badge&logo=readthedocs&logoColor=%23FAC173)](#contenido)

---

### Configurar MySQL como un servicio {#install_as_service}


Nos posicionamos dentro del directorio de instalación e ingresamos a la carpeta bin y ejecutamos los siguientes comandos. **Para llevar a cabo estos pasos es necesario abrir la sesión de CMD con privilegios de administrador**.


El siguiente comando es para asegurarte de no tener ninguna instancia del servidor corriendo actualmente.  
```
mysqladmin.exe -u root shutdown
```

El siguiente comando registra MySQL como servicio.  

```bat
:: Se registra con el nombre pasado como argumento. 
:: Ej mysqld --install "MySQL8"
:: De lo contrario solo con el nombre MySQL por defecto.
mysqld --install
```

![image 17](/img/posts/mysql-install/17.png)


Ahora ya podemos iniciar el servicio o detenerlo desde la línea de comandos, pero para llevar a cabo el proceso tenemos que abrir una nueva ventana como **administrador**.

**Iniciar el servicio con el comando net**
```bat
net start MySQL
```

**Detener el servicio con el comando:**
```bat
net stop MySQL
```

![image 18](/img/posts/mysql-install/18.png)



**Eliminar el servicio**  
```bat
sc delete "MySQL"
```

---

Si quieres apoyarme te agradecería un simple café &#x1f375;	  

[![BuyMeACoffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://www.buymeacoffee.com/9111592)



