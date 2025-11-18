# TEMA 4 
## CONECTORES
es un conjunto de clases y librerias que van a estar entre nuestra base de datos y nuestro programa y son los responsables de traducir las consultas de nuestra aplicacion a una consulta de una base de datos

## DESFASE DEL OBJETO RELACIONAL
en la base de datos almacenamos la informacion en columnas y filas con informacion sencilla asi que tendremos que hacer una traduccion previa con los objetos para la base de datos 

## PROTOCOLOS DE ACCESO A LAS BBDD
para hacer esa conexion a una base de datos de sql tenemos una jdbc,odbc
### JDBC
solo para java
multiplataforma
el mas recomendado para java 
orientado a objetos
### ODBC
para C,C++;JAVA
solo para windows
no recomendado para java 
no orientado a objetos
## COMPONENTES
- API de JDBC: conjunto de librerías (en java.sql y javax.sql) que permiten conectar y consultar bases de datos relacionales desde Java.

- Paquete de pruebas JDBC: verifica que los drivers cumplan los estándares JDBC.

- Gestor JDBC: conecta la aplicación con el driver JDBC, ya sea por conexión directa o mediante un pool de conexiones.

- Puente JDBC-ODBC: permite usar drivers ODBC como si fueran JDBC.

### Arquitecturas:

- Dos capas: la aplicación se conecta directamente a la base de datos.

- Tres capas: la conexión pasa por un middleware que traduce y gestiona las peticiones hacia la base de datos.

postgreSQL->BBDD y usuario ==> DB.EAVER


<img width="1920" height="1200" alt="Captura de pantalla 2025-11-14 103329" src="https://github.com/user-attachments/assets/e7f65eee-a834-4e25-ac86-746fe0c01906" />

<img width="1920" height="1200" alt="Captura de pantalla 2025-11-14 182153" src="https://github.com/user-attachments/assets/c7e3703f-3e6d-489e-804b-7759525bf8d4" />

<img width="1920" height="1200" alt="Captura de pantalla 2025-11-14 182752" src="https://github.com/user-attachments/assets/9d89dc0f-2b1b-42b7-bc67-a750c9b9c166" />
