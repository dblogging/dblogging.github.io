---
layout: post
title: "Instalar MySQL en Ubuntu"
subtitle : "Instalación y configuración"
date: 2022-02-01 23:25:13 -0400
background: "/img/posts/mysql-install/bg.jpg"
categories: "sql"
---

![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white){:height="80" }


**Actualizar el índice de paquetes apt con el siguiente comando:**
```bash
sudo apt update
```


**Instalar el paquete de MySQL con el comando:**
```bash
sudo apt install mysql-server
```
![step1](/img/posts/install_in_ubuntu/01.png)


**Concluida la instalación, el demonio de MySQL se iniciará automáticamente. Para verificarlo usamos el comando:**
```bash
sudo systemctl status mysql
```
![step2](/img/posts/install_in_ubuntu/02.png)


**Ver en que puerto está abierto**
```bash
cat /etc/services | grep mysql
```
En Debian y derivados, el paquete mysql-server incluye el script llamado **mysql_secure_installation**, el cual permite mejorar la seguridad de la instalación por defecto. Es recomendable correr este script en todas las instalaciones de servidores MySQL para sistemas en producción. En resumen nos permite:  
    
- Cambiar la contraseña del usuario "**root**".
- Deshabilitar el acceso remoto para el usuario "**root**".
- Eliminar cuentas de usuario anónimas que pueden ingresar sin necesidad de una contraseña.
- Eliminar la base de datos "**test**" (si existe), y todo privilegio que permita a cualquier usuario el acceso a bases de datos cuyos nombres comienzan con "**test_**".


**Utilizar el script para una configuración segura:**
```bash
sudo mysql_secure_installation
```
La primera pregunta nos solicitará si queremos validar password para conectarnos al servidor sea seguro, si lo deseamos al momento de crear un nuevo usuario en el sistema MySQL nos validará si el password cumple con las condiciones mínimas de seguridad. Si no queremos esto solamente ingresamos **`N`**

![step3](/img/posts/install_in_ubuntu/03.png)

Luego de acuerdo a la opción que ingresemos nos solicitará ingresar el password para el usuario root (Ojo: esto no tendrá efecto hasta que cambiemos el método de autenticación al usuario root de **auth_socket** a  otro complemento). Una vez ingresamos nuestro password, nos preguntará si deseamos remover a los usuarios ánonimos que se crean por defecto junto a la instalación de MySQL, lo mejor es removerlos.  

![step4](/img/posts/install_in_ubuntu/04.png)


Normalmente, a root solo se le debe permitir conectarse desde 'localhost'. Para así asegurar que no puedan adivinar la password de root desde la red. Así que deshabilitamos el logín remoto. 

![step5](/img/posts/install_in_ubuntu/05.png)

Luego nos preguntá si queremos eliminar la base de datos de prueba, esto es opcional. 

![step6](/img/posts/install_in_ubuntu/06.png)

Luego nos pregunta si queremos recargar la tabla de privilegios. Pondremos si (Y).  

![step7](/img/posts/install_in_ubuntu/07.png)


###  Ajustes de autenticación y privilegios de usuario


En los sistemas Ubuntu con MySQL 5.7 (y versiones posteriores), el usuario **root** de MySQL se configura para la autenticación usando el complemento **auth_socket** de manera predeterminada en lugar de una contraseña. Esto en muchos casos proporciona mayor seguridad y utilidad, pero también puede generar complicaciones cuando deba permitir que un programa externo (como phpMyAdmin) acceda al usuario.  

Para usar un password para conectar a MySQL como **root**, deberemos cambiar el método de autenticación de **auth_socket** a otro complemento como **caching_sha2_password** o **mysql_native_password**. Para hacer esto, abrimos la terminal y nos conectamos con privilegio sudo:

```bash
sudo mysql
```

Para ver el método de autenticación utilizado por las cuentas de usuarios de MySQL ejecutamos la siguiente sentencia dentro de la consola de MySQL:  

```bash
SELECT user, authentication_string, plugin, host 
FROM mysql.user;
```

![step 8](/img/posts/install_in_ubuntu/08.png)


Para cambiar el método de autenticación de **root** con una password, utilizaremos el comando **ALTER USER** para cambiar el complemento de autenticación. Lo podriamos de la siguiente manera:


```sql
ALTER USER 'root'@'localhost' 
IDENTIFIED WITH caching_sha2_password BY 'password';
```

O realizar el cambio en dos pasos:


Cambiamos el complemento
```sql
ALTER USER 'root'@'localhost' 
IDENTIFIED WITH mysql_native_password;
```

Cambiamos el password. (La función **`user()`** devuelve al usuario en sessión) 
```sql
  ALTER USER user() IDENTIFIED BY 'Strong_Password;
```


Y por último recargamos la tabla de permisos:  

```sql
FLUSH PRIVILEGES;
```

Otra cosa que te recomiendo es crear un nuevo usuario administrativo con todos los privilegios y acceso a todas las bases de datos:

```sql
GRANT ALL PRIVILEGES ON *.* TO 'admin_user'@'localhost'
IDENTIFIED BY 'very_strong_password';
```


Para desinstalar MySQL con:

```bash
sudo apt-get remove --purge mysql-server mysql-client mysql-common
```

---


Si quieres apoyarme te agradecería un simple café &#x1f375;	  

[![BuyMeACoffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://www.buymeacoffee.com/9111592)



