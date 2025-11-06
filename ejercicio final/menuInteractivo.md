# EJERCICIO MENU INTERACTIVO

Se te ha encargado diseñar un sistema de reviews de películas para el cine de tu pueblo. Por
desgracia, no ha llegado lo último en tecnología y deberás utilizar ficheros de texto plano para
almacenar las reseñas.
Diseñe un programa que cumpla con las siguientes características:
1. Menú interactivo: diseñe un menú de inicio que permita al usuario interactuar con el
sistema.
○ Crear un usuario nuevo.
○ Eliminar un usuario.
○ Añadir una review de un usuario existente.
○ Mostrar las reviews de un usuario existente.
○ Salir.
2. Estructura de directorios:
○ Todo el proyecto se almacenará dentro de una carpeta llamada “cinema2000”.
○ Los datos se almacenarán en una carpeta llamada “reviews”, almacenando las
reseñas de los usuarios en un fichero .txt con el nombre y código del usuario como
nombre del fichero. Por ejemplo, si mi nombre es Guillermo y mi código de usuario
es el [1], el fichero donde se almacenen mis reviews se llamará Guillermo-1.txt.
○ Los datos de los usuarios del sistema se guardarán en un fichero “users” en el que
almacenemos el nombre del usuario, su código y su contraseña.
3. Uso de clases: para la gestión de los datos dentro de su aplicación, implemente las
clases User y Review.
○ La clase User contendrá un nombre, un código y una contraseña.
○ La clase Review contendrá un nombre de película y una calificación
# CinemaReviews
````java 
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 */

package com.nicolas.cinemareviews;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

/**
 *
 * @author Administrator
 */

import java.util.Scanner;

public class CinemaReviews {
    static String usuarioActual = null;
    static String codigoActual = null;

    public static void main(String[] args) {
        File cinema2000 = new File("cinema2000");
        File reviewsDir = new File(cinema2000, "reviews");
        File usersFile = new File(cinema2000, "users.txt");

        if (!cinema2000.exists()) cinema2000.mkdir();
        if (!reviewsDir.exists()) reviewsDir.mkdir();
        try {
            if (!usersFile.exists()) usersFile.createNewFile();
        } catch (IOException e) {
            System.out.println("Error al crear el archivo users.txt: " + e.getMessage());
        }

        Scanner scanner = new Scanner(System.in);
        boolean running = true;

        while (running) {
            System.out.println("\nMenú:");
            System.out.println("0. Iniciar sesión");
            System.out.println("1. Crear un usuario nuevo");
            System.out.println("2. Eliminar un usuario");
            System.out.println("3. Añadir una review");
            System.out.println("4. Mostrar reviews");
            System.out.println("5. Salir");

            if (usuarioActual != null) {
                System.out.println(" Sesión activa: " + usuarioActual + " (Código: " + codigoActual + ")");
            }

            System.out.print("Elige una opción: ");
            int opcion = scanner.nextInt();
            scanner.nextLine(); 

            switch (opcion) {
    case 0:
        iniciarSesion(scanner, usersFile);
        break;
    case 1:
        crearUsuario(scanner, usersFile, reviewsDir);
        break;
    case 2:
        eliminarUsuario(scanner, usersFile, reviewsDir);
        break;
    case 3:
        añadirReview(scanner, reviewsDir);
        break;
    case 4:
        mostrarReviews(reviewsDir);
        break;
    case 5:
        running = false;
        break;
    default:
        System.out.println("Opción no válida.");
        break;
}

        }
    }

    private static void iniciarSesion(Scanner scanner, File usersFile) {
        System.out.print("Nombre: ");
        String nombre = scanner.nextLine();
        System.out.print("Código: ");
        String codigo = scanner.nextLine();
        System.out.print("Contraseña: ");
        String contrasena = scanner.nextLine();

        try (BufferedReader reader = new BufferedReader(new FileReader(usersFile))) {
            String line;
            while ((line = reader.readLine()) != null) {
                User user = User.fromFileLine(line);
                if (user != null && user.getNombre().equals(nombre) &&
                    user.getCodigo().equals(codigo) &&
                    user.getContrasena().equals(contrasena)) {
                    usuarioActual = nombre;
                    codigoActual = codigo;
                    System.out.println("Sesión iniciada correctamente.");
                    return;
                }
            }
           
        } catch (IOException e) {
            System.out.println("Error al iniciar sesión: " + e.getMessage());
        }
    }

    private static void crearUsuario(Scanner scanner, File usersFile, File reviewsDir) {
        try (FileWriter writer = new FileWriter(usersFile, true)) {
            System.out.print("Nombre: ");
            String nombre = scanner.nextLine();
            System.out.print("Código: ");
            String codigo = scanner.nextLine();
            System.out.print("Contraseña: ");
            String contrasena = scanner.nextLine();

            User user = new User(nombre, codigo, contrasena);
            writer.write(user.toFileString() + "\n");

            File userFile = new File(reviewsDir, nombre + "-" + codigo + ".txt");
            userFile.createNewFile();
            System.out.println("Usuario creado correctamente.");
        } catch (IOException e) {
            System.out.println("Error al crear el usuario: " + e.getMessage());
        }
    }

    private static void eliminarUsuario(Scanner scanner, File usersFile, File reviewsDir) {
        System.out.print("Nombre: ");
        String nombre = scanner.nextLine();
        System.out.print("Código: ");
        String codigo = scanner.nextLine();

        try {
            File tempFile = new File(usersFile.getParent(), "temp.txt");
            try (BufferedReader reader = new BufferedReader(new FileReader(usersFile));
                 FileWriter writer = new FileWriter(tempFile)) {
                String line;
                boolean found = false;
                while ((line = reader.readLine()) != null) {
                    if (line.startsWith(nombre + "," + codigo + ",")) {
                        found = true;
                    } else {
                        writer.write(line + "\n");
                    }
                }
                if (found) {
                    usersFile.delete();
                    tempFile.renameTo(usersFile);
                    File userFile = new File(reviewsDir, nombre + "-" + codigo + ".txt");
                    userFile.delete();
                    System.out.println("Usuario eliminado correctamente.");

                    if (nombre.equals(usuarioActual) && codigo.equals(codigoActual)) {
                        usuarioActual = null;
                        codigoActual = null;
                        System.out.println("Sesión cerrada porque el usuario fue eliminado.");
                    }
                } else {
                    System.out.println("Usuario no encontrado.");
                }
            }
        } catch (IOException e) {
            System.out.println("Error al eliminar el usuario: " + e.getMessage());
        }
    }

    private static void añadirReview(Scanner scanner, File reviewsDir) {    
        File userFile = new File(reviewsDir, usuarioActual + "-" + codigoActual + ".txt");
        if (userFile.exists()) {
            try (FileWriter writer = new FileWriter(userFile, true)) {
                System.out.print("Nombre de la película: ");
                String pelicula = scanner.nextLine();
                System.out.print("Calificación (1-10): ");
                int calificacion = scanner.nextInt();
                scanner.nextLine(); 
                System.out.print("Comentario: ");
                String comentario = scanner.nextLine();

                Review review = new Review(pelicula, calificacion, comentario);
                writer.write(review.toFileString() + "\n");
                System.out.println("Review añadida correctamente.");
                System.out.println(review);
            } catch (IOException e) {
                System.out.println("Error al añadir la review: " + e.getMessage());
            }
        } else {
            System.out.println("El archivo de usuario no existe.");
        }
    }

   private static void mostrarReviews(File reviewsDir) {
    if (usuarioActual == null || codigoActual == null) {
        System.out.println("Debes iniciar sesión primero.");
        return;
    }

    File userFile = new File(reviewsDir, usuarioActual + "-" + codigoActual + ".txt");

    System.out.println("Reviews de " + usuarioActual + ":");

    try (BufferedReader reader = new BufferedReader(new FileReader(userFile))) {
        String line;
        while ((line = reader.readLine()) != null) {
            System.out.println(line);
        }
    } catch (IOException e) {
        System.out.println("Error al leer las reviews: " + e.getMessage());
    }
}


}

```` 
# Reviews
```java

/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Class.java to edit this template
 */
package com.nicolas.cinemareviews;

/**
 *
 * @author Administrator
 */
public class Review {
    private String nombrePelicula;
    private int calificacion;
    private String comentario;

    public Review(String nombrePelicula, int calificacion, String comentario) {
        this.nombrePelicula = nombrePelicula;
        this.calificacion = calificacion;
        this.comentario = comentario;
    }

    public String toFileString() {
        return nombrePelicula + " - " + calificacion + " - " + comentario;
    }

    public static Review fromFileLine(String line) {
        String[] partes = line.split(" - ", 3);
        if (partes.length == 3) {
            try {
                int nota = Integer.parseInt(partes[1].trim());
                return new Review(partes[0].trim(), nota, partes[2].trim());
            } catch (NumberFormatException ignored) {}
        }
        return null;
    }

    @Override
    public String toString() {
        return "🎬 " + nombrePelicula + " → ⭐ " + calificacion + "/10\n📝 " + comentario;
    }
}
```
# Users
```java
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Class.java to edit this template
 */
package com.nicolas.cinemareviews;

/**
 *
 * @author Administrator
 */
public class User {
    private String nombre;
    private String codigo;
    private String contrasena;

    public User(String nombre, String codigo, String contrasena) {
        this.nombre = nombre;
        this.codigo = codigo;
        this.contrasena = contrasena;
    }

    public String getNombre() {
        return nombre;
    }

    public String getCodigo() {
        return codigo;
    }

    public String getContrasena() {
        return contrasena;
    }

    public String toFileString() {
        return nombre + "," + codigo + "," + contrasena;
    }

    public static User fromFileLine(String line) {
        String[] partes = line.split(",");
        if (partes.length == 3) {
            return new User(partes[0], partes[1], partes[2]);
        }
        return null;
    }


    @Override
    public String toString() {
        return "Usuario: " + nombre + " (Código: " + codigo + ")";
    }
}

```

