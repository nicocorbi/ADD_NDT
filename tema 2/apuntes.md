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
        PushbackReader reader = new PushbackReader(new FileReader("tema2.txt"), 4); // buffer de 4

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