# Ejercicio Final

Una biblioteca desea organizar digitalmente sus libros y sesiones de lectura. Implementa en Java un programa que realice las siguientes acciones, en orden:

1. Crear el directorio principal Biblioteca.  
   ○ Si ya existen, no deben volver a crearse.  
   ○ Al crear cualquier directorio debe mostrarse su ruta absoluta.

2. Crear dentro de Biblioteca las categorías de libros como subdirectorios:  
   ○ Novela, Poesía, Ciencia, Historia, Arte.  
   ○ Si ya existen, no deben volver a crearse.

3. Crear dentro de cada categoría un archivo catalogo.txt. En este archivo se irá registrando información sobre los libros.

4. Operaciones de escritura y lectura:  
   ○ En Novela/catalogo.txt guardar y leer con bytes: "Don Quijote (1605) - Miguel de Cervantes".  
   ○ En Poesía/catalogo.txt guardar y leer con caracteres: "Veinte poemas de amor (1924) - Pablo Neruda".  
   ○ En Ciencia/catalogo.txt guardar y leer con acceso aleatorio: "El origen de las especies (1859) - Charles Darwin". Luego corregir el año a 1858 sin sobrescribir todo el contenido.

5. Listar todo el contenido de Biblioteca (carpetas y archivos), mostrando sus rutas relativas desde Biblioteca.


```java
package com.nicolas.ejercicioaccesoadatos;

import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.io.RandomAccessFile;

public class EjercicioAccesoaDatos {
    public static void main(String[] args) {
        File biblioteca = new File("Biblioteca");
        if (!biblioteca.exists()) {
            if (biblioteca.mkdir()) {
                System.out.println("Directorio 'Biblioteca' creado correctamente.");
                System.out.println("Ruta absoluta: " + biblioteca.getAbsolutePath());
            } else {
                System.out.println("No se pudo crear el directorio.");
            }
        } else {
            System.out.println("El directorio 'Biblioteca' ya existe.");
        }

        String[] categorias = {"Novela", "Poesía", "Ciencia", "Historia", "Arte"};
        for (String categoria : categorias) {
            File subdirectorio = new File(biblioteca, categoria);
            if (!subdirectorio.exists()) {
                if (subdirectorio.mkdir()) {
                    System.out.println("Subdirectorio '" + categoria + "' creado correctamente.");
                    System.out.println("Ruta absoluta: " + subdirectorio.getAbsolutePath());
                } else {
                    System.out.println("No se pudo crear el subdirectorio '" + categoria + "'.");
                }
            } else {
                System.out.println("El subdirectorio '" + categoria + "' ya existe.");
            }

            File catalogo = new File(subdirectorio, "catalogo.txt");
            try {
                if (catalogo.createNewFile()) {
                    System.out.println("Archivo 'catalogo.txt' creado en '" + categoria + "'.");
                } else {
                    System.out.println("El archivo 'catalogo.txt' ya existe en '" + categoria + "'.");
                }
            } catch (IOException e) {
                System.out.println("Error al crear el archivo 'catalogo.txt' en '" + categoria + "': " + e.getMessage());
            }
        }

        try {
            // Novela: guardar y leer con bytes
            File novelaCatalogo = new File(biblioteca, "Novela/catalogo.txt");
            String novelaTexto = "Don Quijote (1605) - Miguel de Cervantes";
            FileWriter novelaWriter = new FileWriter(novelaCatalogo);
            novelaWriter.write(novelaTexto);
            novelaWriter.close();
            System.out.println("Escrito en Novela/catalogo.txt: " + novelaTexto);

            File poesiaCatalogo = new File(biblioteca, "Poesía/catalogo.txt");
            String poesiaTexto = "Veinte poemas de amor (1924) - Pablo Neruda";
            FileWriter poesiaWriter = new FileWriter(poesiaCatalogo);
            poesiaWriter.write(poesiaTexto);
            poesiaWriter.close();
            System.out.println("Escrito en Poesía/catalogo.txt: " + poesiaTexto);

            File cienciaCatalogo = new File(biblioteca, "Ciencia/catalogo.txt");
            String cienciaTexto = "El origen de las especies (1859) - Charles Darwin";
            RandomAccessFile raf = new RandomAccessFile(cienciaCatalogo, "rw");
            raf.write(cienciaTexto.getBytes());
            System.out.println("Escrito en Ciencia/catalogo.txt: " + cienciaTexto);

            raf.seek(27); 
            raf.write("1858".getBytes());
            raf.close();
            System.out.println("Año corregido en Ciencia/catalogo.txt.");

        } catch (IOException e) {
            System.out.println("Error durante las operaciones de escritura/lectura: " + e.getMessage());
        }

        System.out.println("\nContenido de 'Biblioteca':");
        listarContenido(biblioteca, "");
    }

    public static void listarContenido(File directorio, String indentacion) {
        File[] archivos = directorio.listFiles();
        if (archivos != null) {
            for (File archivo : archivos) {
                System.out.println(indentacion + archivo.getName());
                if (archivo.isDirectory()) {
                    listarContenido(archivo, indentacion + "  ");
                }
            }
        }
    }
}
```


