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
package add_tema5;   // Paquete donde se encuentra esta clase

import java.sql.Connection;   // Necesario para manejar la conexión a la BD
import java.util.Scanner;     // Permite leer datos desde teclado

/**
 * Clase principal del programa
 */
public class ADD_tema5 {

    /**
     * Método main: punto de entrada del programa
     */
    public static void main(String[] args) {

        // Muestra por pantalla los valores leídos desde el archivo XML de configuración
        System.out.println("URL: " + ConfiguracionXML.getUrl());
        System.out.println("Usuario: " + ConfiguracionXML.getUsuario());
        System.out.println("Password: " + ConfiguracionXML.getPassword());

        // Establece la conexión con la base de datos llamando a la clase Conexion
        Connection conn = (Connection) Conexion.conectar();

        // DAOs que gestionan entrenadores y jugadores, usando la misma conexión
        EntrenadorDAO entrenadorDAO = new EntrenadorDAO(conn);
        JugadorDAO jugadorDAO = new JugadorDAO(conn);

        // Scanner para leer la entrada del usuario
        Scanner sc = new Scanner(System.in);

        int opcion;   // Variable para controlar el menú

        // Bucle principal del menú (se repetirá hasta que el usuario pulse 0)
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

            opcion = sc.nextInt();  // Lee la opción
            sc.nextLine();          // Limpia buffer para evitar saltos de línea erróneos

            switch (opcion) {

                case 1:
                    // Añadir entrenador
                    System.out.print("Nombre: ");
                    String nombreE = sc.nextLine();

                    System.out.print("Equipo: ");
                    String equipo = sc.nextLine();   // Solo válido si tu clase y tabla tienen 'equipo'

                    System.out.print("Raza: ");
                    String raza = sc.nextLine();

                    // Crea un nuevo entrenador con su nombre
                    Entrenador e = new Entrenador(nombreE);
                    e.setRaza(raza);      // Establece raza
                    e.setPartidos(0);     // Inicializa partidos jugados

                    // Inserta el entrenador en la BD
                    entrenadorDAO.add(e);
                    break;

                case 2:
                    // Añadir jugador
                    System.out.print("Nombre: ");
                    String nombreJ = sc.nextLine();

                    System.out.print("Posición: ");
                    String pos = sc.nextLine();

                    System.out.print("ID Entrenador: ");
                    int idEnt = sc.nextInt();
                    sc.nextLine();

                    // Crea un nuevo jugador con valores iniciales
                    // puntos = 0, herido = false, idEntrenador = idEnt
                    Jugador j = new Jugador(nombreJ, pos, 0, false, idEnt);

                    // Inserta el jugador en la BD
                    jugadorDAO.add(j);
                    break;

                case 3:
                    // Eliminar entrenador por ID
                    System.out.print("ID Entrenador: ");
                    int idBorrarE = sc.nextInt();

                    entrenadorDAO.delete(idBorrarE);  // Ejecuta el borrado
                    break;

                case 4:
                    // Eliminar jugador por ID
                    System.out.print("ID Jugador: ");
                    int idBorrarJ = sc.nextInt();

                    jugadorDAO.delete(idBorrarJ);     // Ejecuta el borrado
                    break;

                case 5:
                    // Listar entrenadores
                    System.out.println("\nLISTA DE ENTRENADORES:");

                    // Recorre todos los entrenadores obtenidos desde la BD
                    for (Entrenador ent : entrenadorDAO.findAll()) {
                        System.out.println("ID: " + ent.getId() +
                                " | Nombre: " + ent.getNombre() +
                                " | Equipo: " + ent.getEquipo() +  // Solo si existe esa columna
                                " | Raza: " + ent.getRaza());
                    }
                    break;

                case 6:
                    // Listar jugadores pertenecientes a un entrenador
                    System.out.print("ID Entrenador: ");
                    int idLista = sc.nextInt();

                    System.out.println("\nJUGADORES DEL ENTRENADOR " + idLista + ":");

                    // Busca jugadores cuyo entrenador coincida con el ID dado
                    for (Jugador jug : jugadorDAO.findByEntrenador(idLista)) {
                        System.out.println("ID: " + jug.getId() +
                                " | Nombre: " + jug.getNombre() +
                                " | Posición: " + jug.getPosicion() +
                                " | Herido: " + jug.isHerido());
                    }
                    break;

                case 7:
                    // Lesionar jugador (poner herido = true)
                    System.out.print("ID Jugador a lesionar: ");
                    int idLesionar = sc.nextInt();

                    jugadorDAO.lesionar(idLesionar);  // Marca al jugador como lesionado
                    break;
            }

        } while (opcion != 0);   // El menú se repite mientras la opción no sea 0

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

/**
 * Clase encargada de gestionar la conexión con la base de datos.
 * Lee los datos desde el archivo XML y crea la conexión JDBC.
 */
public class Conexion {

    // Variables estáticas que almacenarán los datos de conexión
    private static String url;
    private static String user;
    private static String password;
    private static String driver;

    /**
     * Método que carga los valores del archivo XML usando ConfiguracionXML.
     */
    public static void cargarConfiguracion() {
        // Se obtienen los valores definidos en el XML
        driver = ConfiguracionXML.getDriver();
        url = ConfiguracionXML.getUrl();
        user = ConfiguracionXML.getUsuario();
        password = ConfiguracionXML.getPassword();

        // Información mostrada por consola para verificar que se cargó correctamente
        System.out.println("URL cargada: " + url);
        System.out.println("USER cargado: " + user);
        System.out.println("PASS cargada: " + password);
        System.out.println("DRIVER cargado: " + driver);
    }

    /**
     * Método que crea y devuelve una conexión a la base de datos.
     */
    public static Connection conectar() {
        try {
            // Si la configuración aún no está cargada, se carga en este momento
            if (url == null) {
                cargarConfiguracion();
            }

            // Carga la clase del driver JDBC especificado en el XML
            Class.forName(driver);

            // Devuelve la conexión utilizando DriverManager
            return DriverManager.getConnection(url, user, password);

        } catch (ClassNotFoundException | SQLException e) {
            // En caso de error se muestra un mensaje y la traza
            System.out.println("❌ ERROR al conectar con la base de datos");
            e.printStackTrace();
            return null;  // Devuelve null si no se pudo conectar
        }
    }

    /**
     * Método para cerrar correctamente la conexión.
     */
    public static void cerrar(Connection conn) {
        try {
            // Solo intenta cerrar si la conexión existe y no está cerrada
            if (conn != null && !conn.isClosed()) {
                conn.close();
                System.out.println("✔ Conexión cerrada correctamente.");
            }
        } catch (SQLException e) {
            e.printStackTrace();  // Muestra error si falla al cerrar
        }
    }
}


```
## ConfiguracionXML
```java
package add_tema5;

import java.io.File;                       // Para acceder al archivo config.xml
import javax.xml.parsers.DocumentBuilder;   // Utilizado para construir el parser del XML
import javax.xml.parsers.DocumentBuilderFactory;
import org.w3c.dom.Document;               // Representa el documento XML en memoria
import org.w3c.dom.NodeList;               // Lista de nodos (etiquetas)

/**
 * Clase encargada de leer y cargar los valores del archivo config.xml.
 * La carga se realiza automáticamente gracias al bloque estático.
 */
public class ConfiguracionXML {

    // Constructor vacío (no se utiliza, pero la clase lo incluye)
    public ConfiguracionXML(String configxml) {
    }

    // Variables donde se almacenarán los valores del XML
    private static String url;
    private static String usuario;
    private static String password;
    private static String driver;

    /**
     * 🔥 BLOQUE ESTÁTICO
     * Se ejecuta AUTOMÁTICAMENTE al cargar la clase en memoria.
     * No hace falta crear un objeto de esta clase para que se ejecute.
     */
    static {
        try {
            // Archivo XML de configuración (ruta relativa)
            File file = new File("config.xml");

            // Fábrica y constructor del parser XML
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();

            // Se carga el XML en memoria
            Document doc = builder.parse(file);
            doc.getDocumentElement().normalize();   // Limpia espacios y estructura

            // Obtiene las etiquetas del XML
            NodeList urlList = doc.getElementsByTagName("url");
            NodeList userList = doc.getElementsByTagName("user");
            NodeList passList = doc.getElementsByTagName("password");
            NodeList driverList = doc.getElementsByTagName("driver");

            // Si existe el nodo, se extrae su contenido
            if (urlList.getLength() > 0) 
                url = urlList.item(0).getTextContent();

            if (userList.getLength() > 0) 
                usuario = userList.item(0).getTextContent();

            if (passList.getLength() > 0) 
                password = passList.item(0).getTextContent();

            if (driverList.getLength() > 0) 
                driver = driverList.item(0).getTextContent();

        } catch (Exception e) {
            // Error al leer el archivo XML
            System.err.println("Error al leer el fichero config.xml");
            e.printStackTrace();
        }
    }

    // Métodos getters para obtener los valores cargados del XML

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

/**
 * Clase que representa a un Entrenador dentro del sistema.
 * Contiene sus datos básicos y los métodos para acceder y modificarlos.
 */
public class Entrenador {

    // ======= Atributos del entrenador =======

    private int id;            // Identificador único del entrenador en la BD
    private String nombre;     // Nombre del entrenador
    private String raza;       // Raza (tipo) del entrenador
    private int n_partidos;    // Número de partidos jugados
    private String equipo;     // Nombre del equipo (si existiera en la BD)


    // ======= Constructor =======

    /**
     * Constructor que inicializa solo el nombre del entrenador.
     * IMPORTANTE: aquí hay un pequeño error: equipo nunca se asigna.
     */
    public Entrenador(String nombre) {
        this.nombre = nombre;

        // ⚠️ Esto NO hace nada porque "equipo" está sin inicializar:
        // this.equipo = equipo;  ← equipo es null porque no se recibe por parámetro
        this.equipo = equipo;
    }


    // ======= Getters y Setters =======

    public int getId() { 
        return id; 
    }
    public void setId(int id) { 
        this.id = id; 
    }

    public String getNombre() { 
        return nombre; 
    }
    public void setNombre(String nombre) { 
        this.nombre = nombre; 
    }

    public String getEquipo() { 
        return equipo; 
    }
    public void setEquipo(String equipo) { 
        this.equipo = equipo; 
    }

    public int getPartidos() { 
        return n_partidos; 
    }
    public void setPartidos(int n_partidos) { 
        this.n_partidos = n_partidos; 
    }
    
    public String getRaza() { 
        return raza; 
    }
    public void setRaza(String raza) { 
        this.raza = raza; 
    }
}

```
## EntrenadorDao
```java
package add_tema5;

import java.sql.Connection;        // Para manejar la conexión a la base de datos
import java.sql.PreparedStatement; // Para consultas SQL con parámetros
import java.sql.Statement;         // Para consultas SQL simples
import java.sql.ResultSet;         // Para leer resultados de consultas SELECT
import java.sql.SQLException;      // Para capturar errores SQL
import java.util.ArrayList;        // Para devolver listas de entrenadores

/**
 * DAO (Data Access Object) para la tabla 'entrenador'.
 * Contiene métodos CRUD: crear, leer, actualizar y borrar entrenadores.
 */
public class EntrenadorDAO {

    // Conexión a la base de datos
    private Connection conn;

    /**
     * Constructor: recibe la conexión ya creada.
     */
    public EntrenadorDAO(Connection conn) {
        this.conn = conn;
    }

    /**
     * Inserta un nuevo entrenador en la base de datos.
     */
    public void add(Entrenador e) {
        String sql = "INSERT INTO entrenador(nombre, raza, n_partidos) VALUES (?, ?, ?)";

        // try-with-resources → cierra automáticamente el PreparedStatement
        try (PreparedStatement ps = conn.prepareStatement(sql)) {

            // Se llenan los parámetros del INSERT
            ps.setString(1, e.getNombre());
            ps.setString(2, e.getRaza());
            ps.setInt(3, e.getPartidos());

            // Ejecuta la consulta
            ps.executeUpdate();

        } catch (SQLException ex) {
            ex.printStackTrace();
        }
    }

    /**
     * Obtiene y devuelve todos los entrenadores registrados.
     */
    public ArrayList<Entrenador> findAll() {
        ArrayList<Entrenador> lista = new ArrayList<>();
        String sql = "SELECT * FROM entrenador";

        try (Statement st = conn.createStatement();
             ResultSet rs = st.executeQuery(sql)) {

            // Recorre cada fila del resultado
            while (rs.next()) {

                // Crea un objeto Entrenador con el nombre obtenido
                Entrenador e = new Entrenador(rs.getString("nombre"));

                // Asigna el resto de atributos
                e.setId(rs.getInt("id"));
                e.setRaza(rs.getString("raza"));
                e.setPartidos(rs.getInt("n_partidos"));

                // Añade a la lista
                lista.add(e);
            }

        } catch (SQLException ex) {
            ex.printStackTrace();
        }

        return lista;
    }

    /**
     * Actualiza un entrenador existente en la base de datos.
     */
    public void modify(Entrenador e) {
        String sql = "UPDATE entrenador SET nombre=?, raza=?, n_partidos=? WHERE id=?";

        try (PreparedStatement ps = conn.prepareStatement(sql)) {

            ps.setString(1, e.getNombre());
            ps.setString(2, e.getRaza());
            ps.setInt(3, e.getPartidos());
            ps.setInt(4, e.getId());  // Identificador del entrenador a modificar

            ps.executeUpdate();

        } catch (SQLException ex) {
            ex.printStackTrace();
        }
    }



    /**
     * Elimina un entrenador por su ID.
     */
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
// Indica que esta clase pertenece al paquete 'add_tema5'.

public class Jugador {

    // ============================
    // Atributos privados de la clase
    // ============================

    private int id;                 // Identificador único del jugador en la base de datos
    private String nombre;          // Nombre del jugador
    private String posicion;        // Posición en la que juega (portero, defensa, etc.)
    private boolean herido;         // Indica si el jugador está lesionado
    private int entrenadorId;       // ID del entrenador al que pertenece este jugador (clave foránea)

    // ============================
    // Constructor de la clase
    // ============================

    public Jugador(String nombre, String posicion, int id, boolean herido, int entrenadorId) {
        this.nombre = nombre;           // Asigna el nombre recibido
        this.posicion = posicion;       // Asigna la posición recibida
        this.herido = herido;           // Asigna si está herido o no
        this.entrenadorId = entrenadorId; // Asocia el jugador a un entrenador mediante su ID
        this.id = id;                   // Asigna el ID único del jugador
    }

    // ============================
    // Getters y Setters
    // ============================

    public int getId() { 
        return id;                      // Devuelve el ID del jugador
    }
    public void setId(int id) { 
        this.id = id;                   // Modifica el ID del jugador
    }

    public String getNombre() { 
        return nombre;                  // Devuelve el nombre del jugador
    }
    public void setNombre(String nombre) { 
        this.nombre = nombre;           // Modifica el nombre del jugador
    }

    public String getPosicion() { 
        return posicion;                // Devuelve la posición del jugador
    }
    public void setPosicion(String posicion) { 
        this.posicion = posicion;       // Modifica la posición del jugador
    }

    public boolean isHerido() { 
        return herido;                  // Devuelve si el jugador está herido (true/false)
    }
    public void setHerido(boolean herido) { 
        this.herido = herido;           // Modifica si el jugador está herido
    }

    public int getEntrenadorId() { 
        return entrenadorId;            // Devuelve el ID del entrenador del jugador
    }
    public void setEntrenadorId(int entrenadorId) { 
        this.entrenadorId = entrenadorId;  // Modifica el ID del entrenador asignado
    }
}

```
## JugadorDao
```java
package add_tema5;
// Indica que esta clase pertenece al paquete 'add_tema5'.

import java.sql.Connection;          // Necesario para manejar la conexión con la base de datos
import java.sql.PreparedStatement;   // Para consultas SQL preparadas
import java.sql.ResultSet;           // Para manejar resultados de consultas SELECT
import java.sql.SQLException;        // Para capturar errores SQL
import java.sql.Statement;           // Para ejecutar consultas simples sin parámetros
import java.util.ArrayList;          // Para devolver listas de objetos Jugador

public class JugadorDAO {

    private Connection conn;          // Objeto conexión que usará el DAO para acceder a la BBDD

    // Constructor que recibe la conexión ya creada desde fuera
    public JugadorDAO(Connection conn) {
        this.conn = conn;             // Guarda la conexión en el atributo interno
    }

    // ==================================================
    // 1. Añadir jugador a la base de datos
    // ==================================================
    public void add(Jugador j) {
        // Sentencia SQL con parámetros
        String sql = "INSERT INTO Jugador (nombre, posicion, herido, entrenador_id) VALUES (?, ?, ?, ?)";

        // PreparedStatement cierra automáticamente con try-with-resources
        try (PreparedStatement ps = conn.prepareStatement(sql)) {

            ps.setString(1, j.getNombre());        // Parámetro 1 → nombre
            ps.setString(2, j.getPosicion());      // Parámetro 2 → posición
            ps.setBoolean(3, j.isHerido());        // Parámetro 3 → estado herido
            ps.setInt(4, j.getEntrenadorId());     // Parámetro 4 → ID entrenador

            ps.executeUpdate();                     // Ejecuta la inserción

        } catch (SQLException ex) {
            ex.printStackTrace();                   // Muestra error si ocurre fallo SQL
        }
    }

    // ==================================================
    // 2. Eliminar jugador por ID
    // ==================================================
    public void delete(int id) {
        String sql = "DELETE FROM Jugador WHERE id = ?";

        try (PreparedStatement ps = conn.prepareStatement(sql)) {
            ps.setInt(1, id);                       // Parámetro 1 → ID del jugador a borrar
            ps.executeUpdate();                     // Ejecuta la eliminación
        } catch (SQLException ex) {
            ex.printStackTrace();
        }
    }

    // ==================================================
    // 3. Listar todos los jugadores
    // ==================================================
    public ArrayList<Jugador> findAll() {
        ArrayList<Jugador> lista = new ArrayList<>();   // Lista donde guardaremos los jugadores
        String sql = "SELECT * FROM Jugador";           // Consulta para obtenerlos todos

        try (Statement st = conn.createStatement();     // Statement simple sin parámetros
             ResultSet rs = st.executeQuery(sql)) {     // Ejecuta consulta SELECT

            // Recorremos cada fila del resultado
            while (rs.next()) {

                // Creamos objeto Jugador con los datos recogidos
                Jugador j = new Jugador(
                        rs.getString("nombre"),         // Nombre desde la columna 'nombre'
                        rs.getString("posicion"),       // Posición desde la columna 'posicion'
                        0,                              // En el constructor NO usamos el id, lo ponemos después con setId()
                        rs.getBoolean("herido"),        // Estado herido
                        rs.getInt("entrenador_id")      // ID entrenador
                );

                j.setId(rs.getInt("id"));               // Ahora sí añadimos el ID obtenido
                lista.add(j);                           // Añadimos el jugador a la lista
            }

        } catch (SQLException ex) {
            ex.printStackTrace();
        }

        return lista;                                   // Devolvemos la lista completa
    }

    // ==================================================
    // 4. Listar jugadores filtrados por entrenador
    // ==================================================
    public ArrayList<Jugador> findByEntrenador(int entrenadorId) {
        ArrayList<Jugador> lista = new ArrayList<>();
        String sql = "SELECT * FROM Jugador WHERE entrenador_id = ?"; // Filtrar por entrenador

        try (PreparedStatement ps = conn.prepareStatement(sql)) {

            ps.setInt(1, entrenadorId);                 // Parámetro → ID del entrenador
            ResultSet rs = ps.executeQuery();           // Ejecuta SELECT filtrado

            while (rs.next()) {

                // Construimos cada jugador asociado a ese entrenador
                Jugador j = new Jugador(
                        rs.getString("nombre"),
                        rs.getString("posicion"),
                        0,
                        rs.getBoolean("herido"),
                        entrenadorId                    // Aquí ya sabemos el entrenadorId de entrada
                );

                j.setId(rs.getInt("id"));
                lista.add(j);
            }

        } catch (SQLException ex) {
            ex.printStackTrace();
        }

        return lista;                                   // Devolvemos los jugadores encontrados
    }

    // ==================================================
    // 5. Marcar jugador como lesionado
    // ==================================================
    public void lesionar(int idJugador) {
        String sql = "UPDATE Jugador SET herido = TRUE WHERE id = ?";

        try (PreparedStatement ps = conn.prepareStatement(sql)) {
            ps.setInt(1, idJugador);                    // Establece el ID del jugador
            ps.executeUpdate();                         // Actualiza el registro
        } catch (SQLException ex) {
            ex.printStackTrace();
        }
    }
}

```
<img width="1920" height="1200" alt="Captura de pantalla 2025-11-25 134105" src="https://github.com/user-attachments/assets/02b11241-e7cf-4e4b-a8df-be6206c1bc47" />
        <img width="1920" height="1200" alt="Captura de pantalla 2025-12-02 131316" src="https://github.com/user-attachments/assets/fd5279ec-8da1-4f3d-91bb-60e1dc29af21" />
<img width="1920" height="1200" alt="Captura de pantalla 2025-12-02 131259" src="https://github.com/user-attachments/assets/c296d099-b8a7-4d75-b2d5-29f65547e528" />
<img width="1920" height="1200" alt="Captura de pantalla 2025-11-28 091945" src="https://github.com/user-attachments/assets/f53e6ca3-37ba-4d58-a4f8-9660a2ad44fc" />
<img width="1920" height="1200" alt="Captura de pantalla 2025-11-25 134115" src="https://github.com/user-attachments/assets/e7090c9b-9477-4145-a857-f873c3fa04d2" />