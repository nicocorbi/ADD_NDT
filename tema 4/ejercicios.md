# EJERCICIO
Deberá crear un proyecto de tipo Maven e incluir la dependencia especificado en el siguiente
enlace (Driver JDBC PostgreSQL):
https://mvnrepository.com/artifact/org.postgresql/postgresql/42.7.4
Realice una captura de pantalla con la dependencia añadida.
Parte 2.
El objetivo ahora es establecer una conexión entre un programa escrito en Java y nuestra base de
datos haciendo uso del conector añadido con la dependencia anterior
Una vez establecida la conexión podemos utilizar el siguiente código para mostrar la información
de la BDD:


```java
package tema4_add;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Tema4_ADD {
    
    // URL de conexión a la base de datos PostgreSQL
    private static final String URL = "jdbc:postgresql://localhost:5433/tema4";
    // Usuario de la base de datos
    private static final String USER = "postgres";
    // Contraseña del usuario
    private static final String PASSWORD = "admin";

    public static void main(String[] args) {

        try (Connection conn = DriverManager.getConnection(URL, USER, PASSWORD)) {

            // Si llega aquí significa que la conexión funcionó
            System.out.println("Conexión exitosa a PostgreSQL!");

        } catch (SQLException e) {
            // Si ocurre un error se muestra aquí
            System.out.println("Error al conectar: " + e.getMessage());
        }
    }
}

```

```java
package tema4_addndt; // Declara el paquete donde está esta clase (organiza el código).

// Importaciones de las clases JDBC necesarias:
import java.sql.Connection;     
import java.sql.DriverManager;  
import java.sql.ResultSet;      
import java.sql.SQLException;   
import java.sql.Statement;      

public class Tema4_ADDNDT {     

    private static final String URL = "jdbc:postgresql://localhost:5432/tema4";
    // URL de conexión JDBC:
    // - "jdbc:postgresql://" -> protocolo JDBC para PostgreSQL
    // - "localhost" -> host donde corre el servidor (aquí la máquina local)
    // - "5432" -> puerto del servidor PostgreSQL (por defecto 5432)
    // - "tema4" -> nombre de la base de datos a la que nos conectamos

    private static final String USER = "postgres";
    // Usuario de la base de datos con el que nos autentificamos.

    private static final String PASSWORD = "admin";
    // Contraseña del usuario anterior.

    public static void main(String[] args) { // Punto de entrada de la aplicación.

        // try-with-resources: abre la conexión y el Statement y los cierra automáticamente
        // cuando se sale del bloque (incluso si ocurre una excepción).
        try (Connection conn = DriverManager.getConnection(URL, USER, PASSWORD);
             // DriverManager.getConnection(...) crea y devuelve una Connection usando la URL, usuario y contraseña.

             Statement stmt = conn.createStatement()) {
             // conn.createStatement() crea un objeto Statement para ejecutar SQL sin parámetros.

            System.out.println("Conexión exitosa!");
            // Si llegamos aquí, la conexión se ha establecido correctamente.

            String query = "SELECT * FROM Usuarios";
            // Definimos la consulta SQL que queremos ejecutar (obtener todas las filas de la tabla Usuarios).

            ResultSet rs = stmt.executeQuery(query);
            // Ejecuta la consulta SELECT y devuelve un ResultSet con todas las filas resultado.

            while (rs.next()) {
                // Iteramos sobre cada fila del ResultSet; rs.next() mueve el cursor a la siguiente fila
                // y devuelve true si existe una fila válida.

                int id = rs.getInt("ID");
                // Obtiene el valor de la columna "ID" como entero de la fila actual.

                String username = rs.getString("USERNAME");
                // Obtiene el valor de la columna "USERNAME" como String.

                String password = rs.getString("PASSWORD");
                // Obtiene el valor de la columna "PASSWORD" como String.

                String nombre = rs.getString("NOMBRE");
                // Obtiene el valor de la columna "NOMBRE" como String.

                System.out.println("ID: " + id +
                                   ", Username: " + username +
                                   ", Password: " + password +
                                   ", Nombre: " + nombre);
                // Imprime por consola los valores leídos de la fila actual (formato legible).
            }

        } catch (SQLException e) {
            // Bloque que captura cualquier excepción relacionada con JDBC/SQL ocurrida dentro del try.

            System.out.println("Error: " + e.getMessage());
            // Muestra por consola el mensaje de error para diagnóstico.
        }
        // Al salir del try-with-resources, conn y stmt se cierran automáticamente (conn.close(), stmt.close()).
    }
}

```
<img width="1920" height="1200" alt="Captura de pantalla 2025-12-02 135526" src="https://github.com/user-attachments/assets/41e2633d-7945-44d9-ad9e-1c7014da7501" />
<img width="1920" height="1200" alt="Captura de pantalla 2025-12-04 164136" src="https://github.com/user-attachments/assets/f874db40-20a1-49ed-bc43-5efde89e8a74" />
