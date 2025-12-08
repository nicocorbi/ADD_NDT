## conexion base de datos de neatbeans a dbeaver
## creamos una java class llamada conexion
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Conexion {
    private static final String URL = "jdbc:postgresql://localhost:5433/prueba1";
    private static final String USER = "postgres";
    private static final String PASSWORD = "admin";
    private static Connection conn = null;

    public static Connection getConnection() {
        if (conn == null) {
            try {
                Class.forName("org.postgresql.Driver");
                conn = DriverManager.getConnection(URL, USER, PASSWORD);
                System.out.println("✅ Conexión establecida correctamente");
            } catch (ClassNotFoundException e) {
                System.out.println("❌ No se encontró el driver JDBC");
                e.printStackTrace();
            } catch (SQLException e) {
                System.out.println("❌ Error al conectar con la base de datos");
                e.printStackTrace();
            }
        }
        return conn;
    }

    public static void closeConnection() {
        try {
            if (conn != null) {
                conn.close();
                System.out.println("🔒 Conexión cerrada");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```
## ponemos esto en el main 
```java
public class prueba1 {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        Conexion.getConnection();
        
    }
    
}
```
## ejemplo basico de mandar informacion desde dbaver a la base de datos
```java
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class InsertarJugador {
    public static void main(String[] args) {
        Connection conn = Conexion.getConnection();
        String sql = "INSERT INTO jugador (nombre, posicion) VALUES (?, ?)";

        try (PreparedStatement ps = conn.prepareStatement(sql)) {
            ps.setString(1, "Lionel Messi");
            ps.setString(2, "Delantero");

            ps.executeUpdate();
            System.out.println("✅ Jugador insertado correctamente");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

## ejemplo basico de creacion de una nueva columna desde neatbeans
```java
import java.sql.Connection;
import java.sql.Statement;
import java.sql.SQLException;

public class AgregarColumnaJugador {
    public static void main(String[] args) {
        Connection conn = Conexion.getConnection();
        String sql = "ALTER TABLE jugador ADD COLUMN equipo VARCHAR(100)";

        try (Statement stmt = conn.createStatement()) {
            stmt.executeUpdate(sql);
            System.out.println("✅ Columna 'equipo' añadida correctamente a la tabla jugador");
        } catch (SQLException e) {
            // Si la columna ya existe, PostgreSQL dará error
            System.out.println("⚠️ Error al añadir la columna (¿ya existe tal vez?)");
            e.printStackTrace();
        }
    }
}
```
ejemplo de conexion a traves de un xml que nos ayuda a cambiar si queremos 
<config>
<driver>org.postgresql.Driver</driver>
<url>jdbc:postgresql://localhost:5433/bloodbown</url>
<user>postgres</user>
<password>admin</password>
</config>