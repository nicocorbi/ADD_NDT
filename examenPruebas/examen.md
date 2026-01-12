# crear las tablas en dbeaver
## crear tabla alumnos 
CREATE TABLE alumnos (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    aula INT NOT NULL,
    aprobado BOOLEAN NOT NULL
);
## tabla profesores 
CREATE TABLE profesores (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);
## añadir columna
ALTER TABLE alumnos
ADD COLUMN id_profesor INT;
## actualizar columna
ALTER TABLE alumnos
ADD CONSTRAINT fk_profesor
FOREIGN KEY (id_profesor)
REFERENCES profesores(id);

# archivo XML
```java
<?xml version="1.0" encoding="UTF-8"?>
<database>
    <url>jdbc:postgresql://localhost:5433/examen</url>
    <user>postgres</user>
    <password>admin</password>
</database>
```
# Alumno
```java
package examen;

public class Alumno {

    private int id;
    private String nombre;
    private int aula;
    private boolean aprobado;
    private int idProfesor;
    private String nombreProfesor;

    // Constructor completo (para listar)
    public Alumno(int id, String nombre, int aula, boolean aprobado,
                  int idProfesor, String nombreProfesor) {
        this.id = id;
        this.nombre = nombre;
        this.aula = aula;
        this.aprobado = aprobado;
        this.idProfesor = idProfesor;
        this.nombreProfesor = nombreProfesor;
    }

    // Constructor sin id (para insertar)
    public Alumno(String nombre, int aula, boolean aprobado, int idProfesor) {
        this.nombre = nombre;
        this.aula = aula;
        this.aprobado = aprobado;
        this.idProfesor = idProfesor;
    }

    // Getters
    public int getId() { return id; }
    public String getNombre() { return nombre; }
    public int getAula() { return aula; }
    public boolean isAprobado() { return aprobado; }
    public int getIdProfesor() { return idProfesor; }
    public String getNombreProfesor() { return nombreProfesor; }
}
```
# AlumnoDAO
```java
package examen;

import java.sql.*;
import java.util.ArrayList;

public class AlumnoDAO {

    private Connection conn;

    public AlumnoDAO(Connection conn) {
        this.conn = conn;
    }

    // 🔹 Añadir alumno (CON profesor)
    public void insertar(Alumno a) throws SQLException {
        String sql = """
            INSERT INTO alumnos (nombre, aula, aprobado, id_profesor)
            VALUES (?, ?, ?, ?)
        """;
        PreparedStatement ps = conn.prepareStatement(sql);
        ps.setString(1, a.getNombre());
        ps.setInt(2, a.getAula());
        ps.setBoolean(3, a.isAprobado());
        ps.setInt(4, a.getIdProfesor());
        ps.executeUpdate();
    }

    // 🔹 Borrar alumno
    public void borrar(int id) throws SQLException {
        String sql = "DELETE FROM alumnos WHERE id = ?";
        PreparedStatement ps = conn.prepareStatement(sql);
        ps.setInt(1, id);
        ps.executeUpdate();
    }

    // 🔹 Cambiar aprobado
    public void cambiarAprobado(int id, boolean aprobado) throws SQLException {
        String sql = "UPDATE alumnos SET aprobado = ? WHERE id = ?";
        PreparedStatement ps = conn.prepareStatement(sql);
        ps.setBoolean(1, aprobado);
        ps.setInt(2, id);
        ps.executeUpdate();
    }

    // 🔹 Cambiar aula
    public void cambiarAula(int id, int aula) throws SQLException {
        String sql = "UPDATE alumnos SET aula = ? WHERE id = ?";
        PreparedStatement ps = conn.prepareStatement(sql);
        ps.setInt(1, aula);
        ps.setInt(2, id);
        ps.executeUpdate();
    }

    // 🔹 Listar alumnos con profesor
    public ArrayList<Alumno> listar() throws SQLException {
        ArrayList<Alumno> lista = new ArrayList<>();

        String sql = """
            SELECT a.id, a.nombre, a.aula, a.aprobado, a.id_profesor,
                   p.nombre AS profesor
            FROM alumnos a
            LEFT JOIN profesores p ON a.id_profesor = p.id
        """;

        Statement st = conn.createStatement();
        ResultSet rs = st.executeQuery(sql);

        while (rs.next()) {
            Alumno a = new Alumno(
                rs.getInt("id"),
                rs.getString("nombre"),
                rs.getInt("aula"),
                rs.getBoolean("aprobado"),
                rs.getInt("id_profesor"),
                rs.getString("profesor")
            );
            lista.add(a);
        }
        return lista;
    }
}
```
# Conexion
```java
package examen;

import examen.ConfigDB;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Conexion {

    public static Connection getConexion() throws SQLException {

        ConfigDB config = new ConfigDB();

        return DriverManager.getConnection(
                config.getUrl(),
                config.getUser(),
                config.getPassword()
        );
    }
}
```
# ConfigDB
```java
package examen;

import java.io.InputStream;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

public class ConfigDB {

    private String url;
    private String user;
    private String password;

    public ConfigDB() {
        try {
            InputStream is = getClass()
                    .getClassLoader()
                    .getResourceAsStream("examen/db.xml");

            if (is == null) {
                throw new RuntimeException("No se encuentra db.xml");
            }

            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            Document doc = builder.parse(is);

            Element root = doc.getDocumentElement();

            url = root.getElementsByTagName("url").item(0).getTextContent();
            user = root.getElementsByTagName("user").item(0).getTextContent();
            password = root.getElementsByTagName("password").item(0).getTextContent();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public String getUrl() { return url; }
    public String getUser() { return user; }
    public String getPassword() { return password; }
}
```
# profesor
```java
package examen;

public class Profesor {
    private int id;
    private String nombre;

    public Profesor(int id, String nombre) {
        this.id = id;
        this.nombre = nombre;
    }

    public int getId() { return id; }
    public String getNombre() { return nombre; }
}
```
# profesorDAO
```java
package examen;

import java.sql.*;
import java.util.ArrayList;

public class ProfesorDAO {

    private Connection conn;

    public ProfesorDAO(Connection conn) {
        this.conn = conn;
    }
    public void insertar(String nombre) throws SQLException {
    String sql = "INSERT INTO profesores (nombre) VALUES (?)";
    PreparedStatement ps = conn.prepareStatement(sql);
    ps.setString(1, nombre);
    ps.executeUpdate();
}


    public ArrayList<Profesor> listar() throws SQLException {
        ArrayList<Profesor> lista = new ArrayList<>();
        String sql = "SELECT * FROM profesores";
        Statement st = conn.createStatement();
        ResultSet rs = st.executeQuery(sql);

        while (rs.next()) {
            lista.add(new Profesor(
                rs.getInt("id"),
                rs.getString("nombre")
            ));
        }
        return lista;
    }
}
```
# Main (examen)

```java
package examen;

import java.sql.Connection;
import java.util.Scanner;

public class Examen {

    public static void main(String[] args) {

        try (Connection conn = Conexion.getConexion();
             Scanner sc = new Scanner(System.in)) {

            AlumnoDAO alumnoDAO = new AlumnoDAO(conn);
            ProfesorDAO profesorDAO = new ProfesorDAO(conn);

            int opcion;

            do {
                System.out.println("\n--- MENÚ ALUMNOS ---");
                System.out.println("1. Añadir alumno");
                System.out.println("2. Borrar alumno");
                System.out.println("3. Listar alumnos");
                System.out.println("4. Cambiar aula");
                System.out.println("5. Cambiar aprobado");
                System.out.println("6. Añadir profesor");
                System.out.println("7. Listar profesores");

                System.out.println("0. Salir");
                System.out.print("Elige una opción: ");

                opcion = sc.nextInt();
                sc.nextLine();

                switch (opcion) {
                    case 1 -> añadirAlumno(sc, alumnoDAO, profesorDAO);
                    case 2 -> borrarAlumno(sc, alumnoDAO);
                    case 3 -> listarAlumnos(alumnoDAO);
                    case 4 -> cambiarAula(sc, alumnoDAO);
                    case 5 -> cambiarAprobado(sc, alumnoDAO);
                    case 6 -> añadirProfesor(sc, profesorDAO);
                    case 7 -> listarProfesores(profesorDAO);

                    case 0 -> System.out.println("Saliendo...");
                    default -> System.out.println("Opción no válida");
                }

            } while (opcion != 0);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // ---------- MÉTODOS ----------

    private static void añadirAlumno(Scanner sc, AlumnoDAO alumnoDAO, ProfesorDAO profesorDAO) throws Exception {

        System.out.print("Nombre: ");
        String nombre = sc.nextLine();

        System.out.print("Aula (1 o 2): ");
        int aula = sc.nextInt();

        System.out.print("¿Aprobado? (true/false): ");
        boolean aprobado = sc.nextBoolean();

        System.out.println("\nProfesores disponibles:");
        for (Profesor p : profesorDAO.listar()) {
            System.out.println(p.getId() + " - " + p.getNombre());
        }

        System.out.print("ID del profesor: ");
        int idProfesor = sc.nextInt();
        sc.nextLine();

        Alumno a = new Alumno(nombre, aula, aprobado, idProfesor);
        alumnoDAO.insertar(a);

        System.out.println("Alumno añadido correctamente");
    }

    private static void borrarAlumno(Scanner sc, AlumnoDAO dao) throws Exception {
        System.out.print("ID del alumno: ");
        int id = sc.nextInt();
        dao.borrar(id);
        System.out.println("Alumno borrado");
    }

    private static void listarAlumnos(AlumnoDAO dao) throws Exception {
        for (Alumno a : dao.listar()) {
            System.out.println(
                a.getId() + " | " +
                a.getNombre() +
                " | Aula: " + a.getAula() +
                " | Aprobado: " + a.isAprobado() +
                " | Profesor: " + a.getNombreProfesor()
            );
        }
    }

    private static void cambiarAula(Scanner sc, AlumnoDAO dao) throws Exception {
        System.out.print("ID alumno: ");
        int id = sc.nextInt();

        System.out.print("Nueva aula: ");
        int aula = sc.nextInt();

        dao.cambiarAula(id, aula);
        System.out.println("Aula actualizada");
    }

    private static void cambiarAprobado(Scanner sc, AlumnoDAO dao) throws Exception {
        System.out.print("ID alumno: ");
        int id = sc.nextInt();

        System.out.print("¿Aprobado? (true/false): ");
        boolean aprobado = sc.nextBoolean();

        dao.cambiarAprobado(id, aprobado);
        System.out.println("Estado actualizado");
    }
    private static void añadirProfesor(Scanner sc, ProfesorDAO profesorDAO) throws Exception {
    System.out.print("Nombre del profesor: ");
    String nombre = sc.nextLine();

    profesorDAO.insertar(nombre);
    System.out.println("Profesor añadido correctamente");
}
private static void listarProfesores(ProfesorDAO profesorDAO) throws Exception {
    System.out.println("\n--- PROFESORES ---");
    for (Profesor p : profesorDAO.listar()) {
        System.out.println(p.getId() + " - " + p.getNombre());
    }
}

}
```
