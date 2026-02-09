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
Spring Boot App<img width="1920" height="1200" alt="Captura de pantalla 2026-02-09 181040" src="https://github.com/user-attachments/assets/28bf6c07-9dce-4fb6-af2b-781a599d2392" />

<img width="1920" height="1200" alt="Captura de pantalla 2026-01-30 090836" src="https://github.com/user-attachments/assets/cc498d89-1fd6-425d-98f0-9ae57b512b1a" />
<img width="1920" height="1200" alt="Captura de pantalla 2026-01-30 085504" src="https://github.com/user-attachments/assets/40c2468f-6d9b-4e93-aa46-62728bc48692" />
<img width="1920" height="1200" alt="Captura de pantalla 2026-01-30 092913" src="https://github.com/user-attachments/assets/c31e40f3-5cb3-4c72-bb9d-2c7ce05f37c6" />
<img width="1920" height="1200" alt="Captura de pantalla 2026-01-30 092522" src="https://github.com/user-attachments/assets/4b2e5cc8-d329-4982-9dab-0feb8ab29b0a" />
<img width="1260" height="743" alt="Captura de pantalla 2026-01-30 091829" src="https://github.com/user-attachments/assets/ca576876-f608-4546-9d9c-39e506489132" />
<img width="841" height="785" alt="Captura de pantalla 2026-01-30 091049" src="https://github.com/user-attachments/assets/984cab24-8cc3-45ee-acad-9b40100465f2" />

<img width="1920" height="1200" alt="Captura de pantalla 2026-02-09 181148" src="https://github.com/user-attachments/assets/d7865ec8-85cb-4cf1-b041-cf3623438576" />


<img width="1920" height="1200" alt="Captura de pantalla 2026-02-09 180955" src="https://github.com/user-attachments/assets/ad8bc4a5-d5db-48ac-b8e3-430520e9cbdf" />

