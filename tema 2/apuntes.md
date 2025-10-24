# Tema 2 - flujos
el flujo es una secuencia ordenada de informacion unidireccional que tiene un recurso de entrada o recurso de salida
## ckases de flujos para ficheros
caracteres- filereader filewriter
bytes- fileInputStream, fileOutputStream
## tuberias
mecanismos que permite la comunicacion entre dos hilos de un mismo programa
sirve para poder ramificar una serie de procesos 
caracteres- pipedReader,PipedWriter
bytes- PipedInputStream, PipedOutputStream
n hilo (o thread) es una unidad de ejecución dentro de un programa. Los hilos permiten que un programa realice múltiples tareas de manera concurrente

## Clases de flujos de Arrays
son flujos trabajan con bytes y array
bytes- ByteArrayInputStream
caracteres- CharArrayReader
## Clases de analisis para flujos de datos
bytes-PushBackInputStream,StreamTokenizer
caracteres-LineNumberReader,PushBackReader
para leer informacion y saber si se interpreta adecuadamente.
usamos el trycatch,luego el pushbackreader y debo darle dentro un filereader,primero lee una lectura y luego la imprime
tambien puedo devolverle ese dato al stream y deberian salir ambos con la misma letra,
### ejercicio
modifica el archivo?no , no lo modifica
```java
public class ADDNDT {

    public static void main(String[] args) throws IOException {
        
        PushbackReader pushbackReader = new PushbackReader(new FileReader("tema2.txt"));

        // Leer el primer carácter del archivo
        int data = pushbackReader.read();
        System.out.println("Primer carácter leído: " + (char) data);

        // Devolver el carácter al flujo
        pushbackReader.unread(data);

        // Leer nuevamente el carácter
        data = pushbackReader.read();
        System.out.println("Carácter leído nuevamente: " + (char) data);

        // Cerrar el PushBackReader
        pushbackReader.close();
    }
}
```

PushbackReader es una clase en Java que pertenece al paquete java.io.
Sirve para leer caracteres de un flujo (stream) de entrada, pero con una capacidad especial: puede “devolver” (push back) caracteres al flujo,
de modo que puedan ser leídos de nuevo más tarde.

puedo leer mas de un caracter?si,usando un while podemos hacer que lea mas de un caracter
```java
public class ADDNDT {
    public static void main(String[] args) throws IOException {
        // Paso 1: Crear el archivo con tu nombre
        FileWriter writer = new FileWriter("tema2.txt");
        writer.write("nico");
        writer.close();

        // Paso 2: Leer con PushbackReader
        PushbackReader reader = new PushbackReader(new FileReader("tema2.txt"), 4); // buffer de 4

        int data;
        System.out.println("Lectura y devolución de caracteres:");
        while ((data = reader.read()) != -1) {//lee hasta que no haya mas caracteres
            char c = (char) data;
            System.out.println("Leído: " + c);
            reader.unread(c);//Es un método de la clase PushbackReader que devuelve un carácter al flujo de lectura, como si nunca se hubiera leído.
            System.out.println("Releído: " + (char) reader.read());
        }

        reader.close();
    }
}
```
FileWriter es una clase de Java que te permite escribir texto directamente en un archivo
Un buffer es una zona de memoria temporal que se usa para guardar datos mientras se procesan

que ocurre si hago "unread" sin leer previamente el caracter? no ocurre nada 
```java 
public class ADDNDT {
    public static void main(String[] args) throws IOException {
        // Paso 1: Crear el archivo con tu nombre
        FileWriter writer = new FileWriter("tema2.txt");
        writer.write("nico");
        writer.close();

        // Paso 2: Leer con PushbackReader
        PushbackReader reader = new PushbackReader(new FileReader("tema2.txt"), 4); 

        int data;
        System.out.println("Lectura y devolución de caracteres:");
        while ((data = reader.read()) != -1) {
            
            System.out.println((char)data);
           
        }

        reader.close();
    }
}
```
dentrto del archivo que creemos debe tener nuestro nombre.

¿Qué ocurre si intentas leer antes de que se haya escrito nada?
```java
import java.io.*;

public class ADDNDT {
    public static void main(String[] args) throws IOException {
        // Paso 1: Leer con PushbackReader antes de escribir
        PushbackReader reader = new PushbackReader(new FileReader("tema2.txt"), 4);

        int data;
        System.out.println("Lectura y devolución de caracteres:");
        while ((data = reader.read()) != -1) {
            System.out.println((char) data);
        }

        reader.close();

        // Paso 2: Crear el archivo con tu nombre
        FileWriter writer = new FileWriter("tema2.txt");
        writer.write("nico");
        writer.close();
    }
}


```
si el archivo "tema2.txt" ya existe y tiene contenido, lo leerá sin problema y si el archivo existe pero está vacío, el bucle no imprimirá nada.
¿Se puede conectar un PipedInputStream con varios PipedOutputStream?
No,Un PipedInputStream solo puede conectarse a un único PipedOutputStream

Crea un programa que lea un archivo de texto y cuente:
Cuántas palabras tiene.
Cuántos números aparecen.
Cuántas líneas se procesaron.
```java
import java.io.*;
import java.util.regex.*;

public class ContadorArchivo {
    public static void main(String[] args) throws IOException {
        BufferedReader reader = new BufferedReader(new FileReader("tema2.txt"));

        int lineas = 0;
        int palabras = 0;
        int numeros = 0;

        Pattern numeroPattern = Pattern.compile("\\b\\d+(\\.\\d+)?\\b"); 

        String linea;
        while ((linea = reader.readLine()) != null) {
            lineas++;

            String[] tokens = linea.split("\\s+"); 
            for (String token : tokens) {
                if (!token.isEmpty()) {
                    palabras++;
                    if (numeroPattern.matcher(token).matches()) {
                        numeros++;
                    }
                }
            }
        }

        reader.close();

        System.out.println("Líneas procesadas: " + lineas);
        System.out.println("Palabras encontradas: " + palabras);
        System.out.println("Números encontrados: " + numeros);
    }
}

```
¿Qué ocurre si lees los datos en un orden distinto al que fueron escritos? se bloquea
¿Qué pasa si intentas leer más bytes de los disponibles? se leen solo los disponibles
```java
import java.io.*;

public class LeerBytes {
    public static void main(String[] args) throws IOException {
        // Creamos un archivo con solo 5 bytes
        FileOutputStream fos = new FileOutputStream("datos.txt");
        fos.write("abcde".getBytes()); // 5 bytes: a, b, c, d, e
        fos.close();

        // Intentamos leer 10 bytes
        FileInputStream fis = new FileInputStream("datos.txt");
        byte[] buffer = new byte[10]; 

        int leidos = fis.read(buffer); 

        System.out.println("Bytes leídos: " + leidos);
        System.out.print("Contenido: ");
        for (int i = 0; i < leidos; i++) {
            System.out.print((char) buffer[i]);
        }

        fis.close();
    }
}

```
