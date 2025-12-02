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
package tema4_add;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;
import java.sql.ResultSet;
import java.sql.SQLException;

// Declaración de la clase principal
public class Tema4_ADD {
    
    // URL de conexión a la base de datos PostgreSQL
    private static final String URL = "jdbc:postgresql://localhost:5433/tema4";
    // Usuario de la base de datos
    private static final String USER = "postgres";
    // Contraseña del usuario
    private static final String PASSWORD = "admin";

    // Método principal del programa
    public static void main(String[] args) {

        // try-with-resources: crea la conexión y el statement y los cierra automáticamente
        try (Connection conn = DriverManager.getConnection(URL, USER, PASSWORD);
             Statement stmt = conn.createStatement()) {

            // Si llega aquí significa que la conexión funcionó
            System.out.println("Conexión exitosa!");

            // Consulta SQL para obtener todos los registros de la tabla Usuarios
            String query = "SELECT * FROM Usuarios";
            // Ejecuta la consulta y guarda los resultados en un ResultSet
            ResultSet rs = stmt.executeQuery(query);

            // Recorre cada fila de los resultados obtenidos
            while (rs.next()) {
                // Obtiene el valor de la columna ID como entero
                int id = rs.getInt("ID");
                // Obtiene el nombre de usuario
                String username = rs.getString("USERNAME");
                // Obtiene la contraseña
                String password = rs.getString("PASSWORD");
                // Obtiene el nombre
                String nombre = rs.getString("NOMBRE");

                // Imprime los datos de la fila actual
                System.out.println("ID: " + id + 
                                   ", Username: " + username +
                                   ", Password: " + password + 
                                   ", Nombre: " + nombre);
            }

        } catch (SQLException e) {
            // Si ocurre un error con la conexión o consulta, se muestra aquí
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```
<img width="1920" height="1200" alt="Captura de pantalla 2025-12-02 135526" src="https://github.com/user-attachments/assets/41e2633d-7945-44d9-ad9e-1c7014da7501" />
