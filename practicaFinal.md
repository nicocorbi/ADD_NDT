# practica 
Tarea 1 - Configurar la conexión
De cara a la conexión de nuestra BBDD, deberá tener en cuenta:
● Vamos a externalizar la conexión, por lo que crearemos un archivo “config.xml” con los
parámetros de configuración de nuestra conexión separados por etiquetas (url, user,
password). Un ejemplo de estructura podría ser la siguiente:
<config>
<driver>org.postgresql.Driver</driver>
<url>jdbc:postgresql://localhost:5432/bloodbowl</url>
<user>postgres</user>
<password>1234</password>
</config>
● Deberemos leer dicho archivo .xml de cara a realizar la conexión, por lo que crearemos
una clase que gestione la lectura del mismo y cargue la configuración. Se facilita el archivo
“ConfiguracionXML.java” como plantilla, debiendo completar los apartados señalados con
un TODO en comentarios.
● Recuerde que es importante cerrar la conexión a nuestra BBDD una vez vayamos a
finalizar la ejecución de nuestro programa.

## PASOS
### TAREA 1
✅ PASO 1 — Crear el archivo config.xml

En tu proyecto Java (carpeta src o recursos):

Clic derecho → New → File

Nombre: config.xml

Pega dentro:

<config>
    <driver>org.postgresql.Driver</driver>
    <url>jdbc:postgresql://localhost:5432/bloodbowl</url>
    <user>postgres</user>
    <password>1234</password>
</config>


✔ Este archivo externaliza la conexión
✔ Puedes cambiar el user/pass sin tocar Java

✅ PASO 2 — Tener el driver JDBC en el proyecto

En NetBeans / IntelliJ / Eclipse:

👉 Agrega la librería postgresql-42.xx.jar

Si usas Maven:

<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.7.3</version>
</dependency>
✅ PASO 4 — Usar la configuración en tu main

Ejemplo:

public class Main {
    public static void main(String[] args) {

        ConfiguracionXML config = new ConfiguracionXML();
        config.cargarConfiguracion("config.xml"); // Ruta al archivo

        Connection conn = config.getConnection();

        if (conn != null) {
            System.out.println("Todo OK ✔");
        }

        try {
            conn.close();
            System.out.println("Conexión cerrada correctamente.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
### TAREA 2 – Gestión de la BBDD desde Java: crear tablas, claves foráneas e inserts

A continuación te dejo:

Las sentencias SQL correctas para probarlas en DBeaver primero.

El código Java para crear las tablas con IF NOT EXISTS, claves foráneas y ON DELETE SET NULL.

Cómo insertar datos manualmente para hacer pruebas.

✔️ 1. Sentencias SQL para probar en DBeaver
Tabla Entrenador
CREATE TABLE IF NOT EXISTS Entrenador (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    equipo VARCHAR(100)
);

Tabla Jugador

Incluye la FK con ON DELETE SET NULL:

CREATE TABLE IF NOT EXISTS Jugador (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    posicion VARCHAR(100),
    entrenador_id INTEGER,
    FOREIGN KEY (entrenador_id)
        REFERENCES Entrenador (id)
        ON DELETE SET NULL
);


Prueba estas sentencias en DBeaver para confirmar que funcionan.
Una vez ok → las pasas a Java (siguiente punto).
3. Insertar entrenadores y jugadores manualmente (para pruebas)

Esto se hace directamente en DBeaver (más rápido).
Ejemplo de datos:

Entrenadores
INSERT INTO Entrenador (nombre, equipo)
VALUES
('Juan Gómez', 'Ratas Skaven'),
('Lucía Pérez', 'No Muertos'),
('Carlos Ruiz', 'Elfos Silvanos');

Jugadores
INSERT INTO Jugador (nombre, posicion, entrenador_id)
VALUES
('Skritch', 'Corredor', 1),
('Boneclaw', 'Zombi', 2),
('Leafwind', 'Lanzador', 3),
('Torik', 'Blitzer', 1),
('Gorash', 'Momia', 2);


🔎 Recuerda que entrenador_id debe existir en la tabla Entrenador.
Si borras un entrenador → los jugadores quedan con entrenador_id = NULL.

# Practica Deporte 
## Main --> ADD_tema5
```java
package add_tema5;

import java.sql.Connection;
import java.util.Scanner;

/**
 *
 * @author Administrator
 */
public class ADD_tema5 {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        System.out.println("URL: " + ConfiguracionXML.getUrl());
        System.out.println("Usuario: " + ConfiguracionXML.getUsuario());
        System.out.println("Password: " + ConfiguracionXML.getPassword());
        Connection conn = (Connection) Conexion.conectar();
        EntrenadorDAO entrenadorDAO = new EntrenadorDAO(conn);
        JugadorDAO jugadorDAO = new JugadorDAO(conn);

        Scanner sc = new Scanner(System.in);

        int opcion;

        do {
            System.out.println("\n===== MENÚ PRINCIPAL =====");
            System.out.println("1. Añadir entrenador");
            System.out.println("2. Añadir jugador");
            System.out.println("3. Eliminar entrenador");
            System.out.println("4. Eliminar jugador");
            System.out.println("5. Listar entrenadores");
            System.out.println("6. Listar jugadores de un entrenador");
            System.out.println("7. Lesionar jugador");
            System.out.println("0. Salir");
            System.out.print("Opción: ");
            opcion = sc.nextInt();
            sc.nextLine(); // limpiar buffer

            switch (opcion) {

                case 1:
                    System.out.print("Nombre: ");
                    String nombreE = sc.nextLine();
                    System.out.print("Equipo: ");
                    String equipo = sc.nextLine();
                    System.out.print("Raza: ");
                    String raza = sc.nextLine();

                    Entrenador e = new Entrenador(nombreE);
                    e.setRaza(raza);
                    e.setPartidos(0);

                    entrenadorDAO.add(e);
                    break;

                case 2:
                    System.out.print("Nombre: ");
                    String nombreJ = sc.nextLine();
                    System.out.print("Posición: ");
                    String pos = sc.nextLine();
                    System.out.print("ID Entrenador: ");
                    int idEnt = sc.nextInt();
                    sc.nextLine();

                    Jugador j = new Jugador(nombreJ, pos, 0, false, idEnt);
                    jugadorDAO.add(j);
                    break;

                case 3:
                    System.out.print("ID Entrenador: ");
                    int idBorrarE = sc.nextInt();
                    entrenadorDAO.delete(idBorrarE);
                    break;

                case 4:
                    System.out.print("ID Jugador: ");
                    int idBorrarJ = sc.nextInt();
                    jugadorDAO.delete(idBorrarJ);
                    break;

                case 5:
                    System.out.println("\nLISTA DE ENTRENADORES:");
                    for (Entrenador ent : entrenadorDAO.findAll()) {
                        System.out.println("ID: " + ent.getId() +
                                " | Nombre: " + ent.getNombre() +
                                " | Equipo: " + ent.getEquipo() +
                                " | Raza: " + ent.getRaza());
                    }
                    break;

                case 6:
                    System.out.print("ID Entrenador: ");
                    int idLista = sc.nextInt();
                    System.out.println("\nJUGADORES DEL ENTRENADOR " + idLista + ":");
                    for (Jugador jug : jugadorDAO.findByEntrenador(idLista)) {
                        System.out.println("ID: " + jug.getId() +
                                " | Nombre: " + jug.getNombre() +
                                " | Posición: " + jug.getPosicion() +
                                " | Herido: " + jug.isHerido());
                    }
                    break;

                case 7:
                    System.out.print("ID Jugador a lesionar: ");
                    int idLesionar = sc.nextInt();
                    jugadorDAO.lesionar(idLesionar);
                    break;
            }

        } while (opcion != 0);

        System.out.println("Programa finalizado.");
    }
    
}
      
```
## Conexion
```java
package add_tema5;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Conexion {

    private static String url;
    private static String user;
    private static String password;
    private static String driver;

    public static void cargarConfiguracion() {
        driver = ConfiguracionXML.getDriver();
        url = ConfiguracionXML.getUrl();
        user = ConfiguracionXML.getUsuario();
        password = ConfiguracionXML.getPassword();

        System.out.println("URL cargada: " + url);
        System.out.println("USER cargado: " + user);
        System.out.println("PASS cargada: " + password);
        System.out.println("DRIVER cargado: " + driver);
    }

    public static Connection conectar() {
        try {
            if (url == null) {
                cargarConfiguracion();
            }

            Class.forName(driver);
            return DriverManager.getConnection(url, user, password);

        } catch (ClassNotFoundException | SQLException e) {
            System.out.println("❌ ERROR al conectar con la base de datos");
            e.printStackTrace();
            return null;
        }
    }

    public static void cerrar(Connection conn) {
        try {
            if (conn != null && !conn.isClosed()) {
                conn.close();
                System.out.println("✔ Conexión cerrada correctamente.");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```
## ConfiguracionXML
```java
package add_tema5;

import java.io.File;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.w3c.dom.Document;
import org.w3c.dom.NodeList;

public class ConfiguracionXML {

    public ConfiguracionXML(String configxml) {
    }

    //private static String driver;
    private static String url;
    private static String usuario;
    private static String password;
    private static String driver;

    // Bloque estático para cargar la configuración al iniciar la clase sin necesidad de crear un objeto de la clase
    static {
        try {
            // TODO: Establecer la ruta relativa del fichero XML (ya resuelto)
            File file = new File("config.xml");
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            Document doc = builder.parse(file);
            doc.getDocumentElement().normalize();

            // TODO: leer el contenido del fichero XML tal y como vimos en el tema 3
            NodeList urlList = doc.getElementsByTagName("url");
            NodeList userList = doc.getElementsByTagName("user");
            NodeList passList = doc.getElementsByTagName("password");
            NodeList driverList = doc.getElementsByTagName("driver");

            if (urlList.getLength() > 0) url = urlList.item(0).getTextContent();
            if (userList.getLength() > 0) usuario = userList.item(0).getTextContent();
            if (passList.getLength() > 0) password = passList.item(0).getTextContent();
            if (driverList.getLength() > 0) driver = driverList.item(0).getTextContent();

        } catch (Exception e) {
            System.err.println("Error al leer el fichero config.xml");
            e.printStackTrace();
        }
    }
    public static String getDriver() {
        return driver;
    }
    public static String getUrl() {
        return url;
    }

    public static String getUsuario() {
        return usuario;
    }

    public static String getPassword() {
        return password;
    }
}
```
## Entrenador
```java
package add_tema5;

public class Entrenador {

    private int id;
    private String nombre;
    private String raza;
    private int n_partidos;
    private  String equipo;

    // Constructor

    public Entrenador(String nombre) {
        this.nombre = nombre;
        this.equipo = equipo;
    }

    // Getters y Setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }

    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }

    public String getEquipo() { return equipo; }
    public void setEquipo(String equipo) { this.equipo = equipo; }

    public int getPartidos() { return n_partidos; }
    public void setPartidos(int n_partidos) { this.n_partidos = n_partidos; }
    
    public String getRaza() { return raza; }
    public void setRaza(String raza) { this.raza = raza; }
    
}
```
## EntrenadorDao
```java
package add_tema5;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.Statement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;

public class EntrenadorDAO {
private Connection conn;

public EntrenadorDAO(Connection conn) {
    this.conn = conn;
}

// Crear entrenador
public void add(Entrenador e) {
    String sql = "INSERT INTO entrenador(nombre, raza, n_partidos) VALUES (?, ?, ?)";
    try (PreparedStatement ps = conn.prepareStatement(sql)) {
        ps.setString(1, e.getNombre());
        ps.setString(2, e.getRaza());
        ps.setInt(3, e.getPartidos());
        ps.executeUpdate();
    } catch (SQLException ex) {
        ex.printStackTrace();
    }
}

// Leer todos los entrenadores
public ArrayList<Entrenador> findAll() {
    ArrayList<Entrenador> lista = new ArrayList<>();
    String sql = "SELECT * FROM entrenador";
    try (Statement st = conn.createStatement();
         ResultSet rs = st.executeQuery(sql)) {
        while (rs.next()) {
            Entrenador e = new Entrenador(rs.getString("nombre"));
            e.setId(rs.getInt("id"));
            e.setRaza(rs.getString("raza"));
            e.setPartidos(rs.getInt("n_partidos"));
            lista.add(e);
        }
    } catch (SQLException ex) {
        ex.printStackTrace();
    }
    return lista;
}

// Modificar entrenador
public void modify(Entrenador e) {
    String sql = "UPDATE entrenador SET nombre=?, raza=?, n_partidos=? WHERE id=?";
    try (PreparedStatement ps = conn.prepareStatement(sql)) {
        ps.setString(1, e.getNombre());
        ps.setString(2, e.getRaza());
        ps.setInt(3, e.getPartidos());
        ps.setInt(4, e.getId());
        ps.executeUpdate();
    } catch (SQLException ex) {
        ex.printStackTrace();
    }
}

// Borrar entrenador
public void delete(int id) {
    String sql = "DELETE FROM entrenador WHERE id=?";
    try (PreparedStatement ps = conn.prepareStatement(sql)) {
        ps.setInt(1, id);
        ps.executeUpdate();
    } catch (SQLException ex) {
        ex.printStackTrace();
    }
}

}
```
## Jugador
```java
package add_tema5;

public class Jugador {

    private int id;
    private String nombre;
    private String posicion;
    private boolean herido;
    private int entrenadorId;

    // Constructor

    public Jugador(String nombre, String posicion, int id, boolean herido, int entrenadorId) {
        this.nombre = nombre;
        this.posicion = posicion;
        this.herido = herido;
        this.entrenadorId = entrenadorId;
        this.id = id;
    }

    // Getters y Setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }

    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }

    public String getPosicion() { return posicion; }
    public void setPosicion(String posicion) { this.posicion = posicion; }

    public boolean isHerido() { return herido; }
    public void setHerido(boolean herido) { this.herido = herido; }

    public int getEntrenadorId() { return entrenadorId; }
    public void setEntrenadorId(int entrenadorId) { this.entrenadorId = entrenadorId; }
}
```
## JugadorDao
```java
package add_tema5;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;

public class JugadorDAO {

    private Connection conn;

    public JugadorDAO(Connection conn) {
        this.conn = conn;
    }

    // 1. Añadir jugador
    public void add(Jugador j) {
        String sql = "INSERT INTO Jugador (nombre, posicion, herido, entrenador_id) VALUES (?, ?, ?, ?)";
        try (PreparedStatement ps = conn.prepareStatement(sql)) {
            ps.setString(1, j.getNombre());
            ps.setString(2, j.getPosicion());
            ps.setBoolean(3, j.isHerido());
            ps.setInt(4, j.getEntrenadorId());
            ps.executeUpdate();
        } catch (SQLException ex) {
            ex.printStackTrace();
        }
    }

    // 2. Eliminar jugador
    public void delete(int id) {
        String sql = "DELETE FROM Jugador WHERE id = ?";
        try (PreparedStatement ps = conn.prepareStatement(sql)) {
            ps.setInt(1, id);
            ps.executeUpdate();
        } catch (SQLException ex) {
            ex.printStackTrace();
        }
    }

    // 3. Listar todos los jugadores
    public ArrayList<Jugador> findAll() {
        ArrayList<Jugador> lista = new ArrayList<>();
        String sql = "SELECT * FROM Jugador";

        try (Statement st = conn.createStatement();
             ResultSet rs = st.executeQuery(sql)) {

            while (rs.next()) {
                Jugador j = new Jugador(
                        rs.getString("nombre"),
                        rs.getString("posicion"),
                        0,
                        rs.getBoolean("herido"),
                        rs.getInt("entrenador_id")
                );

                j.setId(rs.getInt("id"));
                lista.add(j);
            }

        } catch (SQLException ex) {
            ex.printStackTrace();
        }

        return lista;
    }

    // 4. Obtener jugadores asociados a un entrenador
    public ArrayList<Jugador> findByEntrenador(int entrenadorId) {
        ArrayList<Jugador> lista = new ArrayList<>();
        String sql = "SELECT * FROM Jugador WHERE entrenador_id = ?";

        try (PreparedStatement ps = conn.prepareStatement(sql)) {
            ps.setInt(1, entrenadorId);
            ResultSet rs = ps.executeQuery();

            while (rs.next()) {
                Jugador j = new Jugador(
                        rs.getString("nombre"),
                        rs.getString("posicion"),
                        0,
                        rs.getBoolean("herido"),
                        entrenadorId
                );
                j.setId(rs.getInt("id"));
                lista.add(j);
            }

        } catch (SQLException ex) {
            ex.printStackTrace();
        }

        return lista;
    }

    // 5. Lesionar jugador
    public void lesionar(int idJugador) {
        String sql = "UPDATE Jugador SET herido = TRUE WHERE id = ?";
        try (PreparedStatement ps = conn.prepareStatement(sql)) {
            ps.setInt(1, idJugador);
            ps.executeUpdate();
        } catch (SQLException ex) {
            ex.printStackTrace();
        }
    }
}
```