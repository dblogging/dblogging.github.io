---
layout: post
title: "Postgre - ZIP"
subtitle : "Instalación y configuración"
date: 2022-02-03 22:25:13 -0400
background: "/img/posts/mysql-install/bg.jpg"
categories: "sql"
---


![Postgre](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white)


## INSTALACIÓN MANUAL DESDE EL ZIP 

 
**&#x1f538; Paso 1:**   
Descargamos el archivo zip<a href="https://www.enterprisedb.com/download-postgresql-binaries" target="_blank"> Aquí &#x1f4c1;</a>

**&#x1f538; Paso 2:**  
Extraemos el contenido del archivo zip (en mi caso lo voy a descomprimir en la siguiente ubicación "**C:\pgsql_14**)

![extract](/img/posts/postgre-install/extract_zip_01.png){:height="300px"}

---


**&#x1f538; Paso 3:**  
Crear una carpeta donde se almacenarán las configuraciones de nuestro servidor (en mi caso lo voy a descomprimir en la siguiente ubicación "**C:\pgsql_data**)

![extract2](/img/posts/postgre-install/extract_zip_02.png){:height="300px"}


**&#x1f538; Paso 4:**  
Configurar el usuario, la contraseña, encriptación, la codificación para la base de datos
Nos cambiamos al directorio bin (en mi caso es "**C:\pgsql_14\bin**"). Y ejecutamos el siguiente comando:  

```cmd
initdb.exe -D C:\pgsql_data -U postgres -W -E UTF8 -A scram-sha-256
```
* **-D**: especifique el directorio de almacenamiento del clúster de bases de datos en mi caso **(C:\pgsql_data)**.
* **-U postgres**: crea al superusuario como **postgres**.
* **-W**: solicita la contraseña del superusuario.
* **-E UTF8**: crea la base de datos con codificación UTF-8
* **-A scram-sha-256**: habilita la autenticación de contraseña.  
     
![postgres_init](/img/posts/postgre-install/init_db.png)

---


**&#x1f538; Paso 5:**  

Iniciar y detener el servidor PostgreSQL
 
* Para ver el estado del servidor y ver si es encuentra en ejecución o no:  
```bash
pg_ctl.exe -D C:\pgsql_data -l logfile status
# Otra opción es la herramienta pg_isready
pg_isready.exe
```
* Para iniciar el servidor ejecute el comando:  
```cmd
pg_ctl.exe -D C:\pgsql_data -l logfile start
```
* Para detener el servidor: 
```cmd
pg_ctl.exe -D C:\pgsql_data stop
```
* Para reiniciar el servidor: 
```cmd
pg_ctl.exe -D C:\pgsql_data stop
```
>Nota: cualquier acción con el servidor es obligatorio indicar el directorio de datos

---

**&#x1f538; Paso 6:**  

Registrar como servicio de Windows

* Abrimos una sesión como administrador y ejecutamos el comando: 
```cmd
pg_ctl.exe register -D C:\pgsql_data -N "postgres14"
```
![install service](/img/posts/postgre-install/register_as_service_01.png)
    
* Para ejecutar después el programa cliente **psql** desde cualquier ubicación establecemos esa variable entorno puede ser con CMD normal o como administrador: 
```cmd
setx PATH "%path%;"C:\pgsql_14\bin\
```
![set_environ](/img/posts/postgre-install/set_environ_variable.png)
    
* Para eliminar el servicio abrimos una sesión CMD como administrador y ejecutamos el comando: 
```cmd
sc delete postgres14
```
---

**&#x1f538; Paso 7:**  

Iniciar sesión en el servidor PostgreSQL

- Usando el cliente **psql.exe** para conectarnos a nuestro servidor. Lo siguiente es llamar al programa e iniciar sesión indicando el usuario y luego nos pedirá el password.  
```cmd
psql -U postgres
```

- Cuando se le solicite la contraseña, ingrese la contraseña que configuró durante la instalación. El prompt nos indica que estamos conetado con éxito y listo para realizar sentencias SQL.
![connection](/img/posts/postgre-install/connection_01.png)

---

## Operaciones básicas en PostgreSQL


para listar los usuarios, use el comando **`\du`**  
para enumerar todas las bases de datos, use el comando **`\list`** o **`\l`**  
para salir solo escribimos **`exit`** o **`\q`**
![postgres_query](/img/posts/postgre-install/create_and_exit.png)
para cambiar a una nueva base de datos, use el comando **`\connect <database>`** o **`\c <database name>`**   
para mostrar tablas de una base de datos, use el comando **`\dt`** o **`\dt+`**
![postgres_query2](/img/posts/postgre-install/create_and_list_table.png)  
para realizar una copia de seguridad o un volcado de la base de datos, use la herramienta pg_dump:  
```cmd
pg_dump.exe -U postgres -d <database name> -f <path>\backup.sql
```
![postgres_bkp](/img/posts/postgre-install/pg_dump_bkp.png)
![postgres_bkp](/img/posts/postgre-install/pg_dump_bkp2.png)  
para importar un archivo `.pgsql` o `.sql` existente al servidor de la base de datos, use el siguiente comando:
```cmd
psql.exe -h <hostname> -U postgres < <path>\backup.sql
```
![postgres_import](/img/posts/postgre-install/pgsql_import_script.png)

---

Si quieres apoyarme te agradecería un simple café &#x1f375;	  

[![BuyMeACoffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://www.buymeacoffee.com/9111592)
