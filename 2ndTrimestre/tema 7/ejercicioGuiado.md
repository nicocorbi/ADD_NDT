# 📘 PRÁCTICA 7 – APUNTES GUIADOS  
## PostgreSQL + DBeaver + Spring Boot (Eclipse)

primero ejecutamos el comando en server de power shell "psql postgres" si tenemos desdecargado ya previamente postgres segundo en el power shell, despues de eso ponemos postgres=# CREATE USER postgres WITH PASSWORD 'postgres'; en dbeaver le damos a iniciar conexion y añadimos el nombre de nuestra database, añadir el nombre de usuario (postgres) y la contraseña "admin" lo demas , se deja igual excepto el puerto , que hay que cambiarlo al 33 añadimos y rellenamos los camnpos del sproing boot junto con las otras 3 dependencias entrando en la pagina "https://start.spring.io/"

## 🔹 PARTE 1: PostgreSQL

### 1️⃣ Comprobar que PostgreSQL funciona
Abrimos **PowerShell** y ejecutamos:

```bash
psql postgres
```
Si todo está correcto, veremos:

Código
postgres=#
### 2️⃣ Crear usuario en PostgreSQL
En la consola de PostgreSQL ejecutamos:
```bash
sql
CREATE USER postgres WITH PASSWORD 'postgres';
```
### 3️⃣ Crear base de datos
Creamos la base de datos para la práctica:
```bash
sql
CREATE DATABASE acceso_a_datos;
```
🔹 PARTE 2: DBeaver
### 4️⃣ Crear conexión a PostgreSQL en DBeaver
Abrimos DBeaver

Pulsamos Nueva conexión

Elegimos PostgreSQL

Rellenamos:

Código
Database: acceso_a_datos
Username: postgres
Password: admin
Puerto: 33
👉 Todo lo demás por defecto.

Si la conexión es correcta, aparecerá en el panel izquierdo.

🔹 PARTE 3: Spring Boot (Eclipse)
### 5️⃣ Crear proyecto Spring Boot
Entramos en:

Código
https://start.spring.io/
Configuración recomendada:

Código
Project: Maven
Language: Java
Spring Boot: por defecto
Group: com.example
Artifact: practica7
Packaging: Jar
Java: 17
Dependencias necesarias:

Spring Web

Spring Data JPA

PostgreSQL Driver

Spring Boot DevTools

Descargamos el proyecto e importamos en Eclipse como Maven Project.

🔹 PARTE 4: Configuración application.properties
Abrimos:

Código
src/main/resources/application.properties
Pegamos:

properties
spring.datasource.url=jdbc:postgresql://localhost:33/acceso_a_datos
spring.datasource.username=postgres
spring.datasource.password=admin

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
🔹 EJECUCIÓN DEL PROYECTO EN ECLIPSE
Para ejecutar:

Click derecho sobre el proyecto

Run As

Spring Boot App