# 📘 TEMA 1 — Introducción al manejo de ficheros (Acceso a datos)

---

## 🧩 1. Conceptos básicos

Un **fichero** es una **sucesión de bits** almacenada en un medio físico (disco duro, SSD, etc.).

### 🔸 Tipos de ficheros
- 📝 **Texto (ASCII):** contienen líneas de texto legible.
- 💾 **Binarios:** almacenan datos en formato binario (no legible por humanos).

---

## 🗂️ 2. Clase `File` (java.io.File)

Representa un archivo o directorio en el sistema.  
📌 *No gestiona el contenido, solo la ruta y las operaciones básicas sobre el archivo.*

### ⚙️ Constructor
```java
File f = new File("C:/Users/Usuario/Desktop/archivo.txt");
🧠 Métodos principales de la clase File
🧱 Acción	🧩 Método	🧪 Ejemplo
Crear fichero	createNewFile()	f.createNewFile();
Borrar fichero	delete()	f.delete();
Crear carpeta	mkdir() / mkdirs()	f.mkdir();
Comprobar si existe	exists()	f.exists();
Obtener nombre	getName()	f.getName();
Obtener ruta relativa	getPath()	f.getPath();
Obtener ruta absoluta	getAbsolutePath()	f.getAbsolutePath();
Obtener directorio padre	getParent()	f.getParent();
Listar archivos de un directorio	listFiles()	f.listFiles();
Comprobar permisos	canRead() / canWrite()	f.canRead();

🧩 3. Tipos de acceso a ficheros
🗂️ Tipo	💬 Descripción
Secuencial	Se lee o escribe de principio a fin, byte a byte o carácter a carácter.
Aleatorio	Permite saltar a cualquier posición dentro del fichero.

💾 4. Acceso secuencial a ficheros
📥 Lectura binaria — FileInputStream
java
Copiar código
import java.io.FileInputStream;
import java.io.IOException;

public class LeerBinario {
    public static void main(String[] args) {
        try (FileInputStream in = new FileInputStream("datos.bin")) {
            int dato;
            while ((dato = in.read()) != -1) {
                System.out.print((char) dato);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
📤 Escritura binaria — FileOutputStream
java
Copiar código
import java.io.FileOutputStream;
import java.io.IOException;

public class EscribirBinario {
    public static void main(String[] args) {
        try (FileOutputStream out = new FileOutputStream("salida.txt")) {
            String texto = "Hola Mundo desde Java";
            out.write(texto.getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
📥 Lectura de texto — FileReader
java
Copiar código
import java.io.FileReader;
import java.io.IOException;

public class LeerTexto {
    public static void main(String[] args) {
        try (FileReader r = new FileReader("texto.txt")) {
            int c;
            while ((c = r.read()) != -1) {
                System.out.print((char) c);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
📤 Escritura de texto — FileWriter
java
Copiar código
import java.io.FileWriter;
import java.io.IOException;

public class EscribirTexto {
    public static void main(String[] args) {
        try (FileWriter w = new FileWriter("texto.txt")) {
            w.write("Ejemplo de escritura de texto en un archivo");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
🎯 5. Acceso aleatorio — RandomAccessFile
Permite leer o escribir en cualquier punto del archivo sin seguir el orden secuencial.

java
Copiar código
import java.io.RandomAccessFile;
import java.io.IOException;

public class AccesoAleatorio {
    public static void main(String[] args) {
        try (RandomAccessFile raf = new RandomAccessFile("datos.txt", "rw")) {
            raf.seek(10); // Mueve el puntero a la posición 10
            raf.writeBytes("Hola desde posición 10");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
🧩 Modos de apertura:

"r" → Solo lectura

"rw" → Lectura y escritura

"rwd" → Lectura/escritura (sincroniza datos)

"rws" → Lectura/escritura (sincroniza datos y metadatos)

📗 TEMA 2 — Flujos (Streams) y acceso a datos
💡 1. Concepto de flujo (Stream)
Un stream es una secuencia ordenada de información que puede ser:

🔹 Entrada → lectura de datos

🔹 Salida → escritura de datos

Son unidireccionales (no leen y escriben al mismo tiempo).

🧱 2. Tipos de flujos (Streams)
🔸 Flujos basados en ficheros
Tipo	Clases
Bytes	FileInputStream, FileOutputStream
Caracteres	FileReader, FileWriter

🔸 Flujos basados en tuberías (pipes)
Permiten comunicación entre hilos (threads) del mismo proceso.

Tipo	Clases
Bytes	PipedInputStream, PipedOutputStream
Caracteres	PipedReader, PipedWriter

📌 Ejemplo de uso:

Un hilo escribe con PipedOutputStream

Otro hilo lee con PipedInputStream

🔸 Flujos basados en arrays
Permiten leer o escribir directamente sobre arrays de bytes o caracteres.

Tipo	Clases
Bytes	ByteArrayInputStream, ByteArrayOutputStream
Caracteres	CharArrayReader, CharArrayWriter

🧪 Ejemplo
java
Copiar código
import java.io.CharArrayWriter;

public class ArrayWriterExample {
    public static void main(String[] args) {
        CharArrayWriter writer = new CharArrayWriter();
        writer.write("Hola mundo");
        char[] datos = writer.toCharArray();
        System.out.println(datos);
    }
}
🔸 Flujos de análisis (análisis de datos)
Sirven para interpretar o analizar los datos que fluyen.

Clase	Función
PushbackInputStream / PushbackReader	Permiten "devolver" un dato leído (retroceso).
StreamTokenizer	Divide texto en tokens (palabras, números, símbolos).
LineNumberReader	Lee texto y cuenta líneas automáticamente.

Ejemplo con StreamTokenizer
java
Copiar código
import java.io.FileReader;
import java.io.StreamTokenizer;
import java.io.IOException;

public class TokenizerExample {
    public static void main(String[] args) {
        try (FileReader fr = new FileReader("texto.txt")) {
            StreamTokenizer st = new StreamTokenizer(fr);
            while (st.nextToken() != StreamTokenizer.TT_EOF) {
                if (st.ttype == StreamTokenizer.TT_WORD)
                    System.out.println("Palabra: " + st.sval);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
🔸 Flujos para tratamiento de información
Permiten leer y escribir tipos primitivos de Java (int, float, double, etc.)

Clase	Función
DataInputStream / DataOutputStream	Leer o escribir datos primitivos de Java

Ejemplo
java
Copiar código
import java.io.DataOutputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class DataStreamExample {
    public static void main(String[] args) {
        try (DataOutputStream out = new DataOutputStream(new FileOutputStream("datos.dat"))) {
            out.writeInt(25);
            out.writeDouble(3.14);
            out.writeUTF("Texto guardado");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
🧾 RESUMEN GLOBAL
Categoría	Clase	Tipo de dato	Función principal
Archivo simple	File	—	Manejar rutas y ficheros
Lectura binaria	FileInputStream	Bytes	Leer archivos binarios
Escritura binaria	FileOutputStream	Bytes	Escribir archivos binarios
Lectura texto	FileReader	Caracteres	Leer archivos de texto
Escritura texto	FileWriter	Caracteres	Escribir archivos de texto
Acceso aleatorio	RandomAccessFile	Bytes	Acceso a posiciones concretas
Comunicación entre hilos	PipedInput/OutputStream	Bytes	Comunicación entre threads
Lectura desde array	ByteArrayInputStream	Bytes	Leer desde memoria
Análisis de texto	StreamTokenizer	Texto	Detectar palabras y números
Datos primitivos	DataInput/OutputStream	Primitivos	Leer/escribir tipos básicos

🧠 CONSEJOS FINALES PARA EL EXAMEN
✅ Entrada = lectura → Input / Reader
✅ Salida = escritura → Output / Writer
✅ Reader/Writer → caracteres (texto)
✅ InputStream/OutputStream → bytes (binario)
✅ Cerrar siempre los flujos (.close())
✅ Usar try-with-resources para seguridad:

java
Copiar código
try (FileWriter w = new FileWriter("archivo.txt")) {
    w.write("Texto seguro");
} catch (IOException e) {
    e.printStackTrace();
}
edad INT

<config>
<driver>org.postgresql.Driver</driver>
<url>jdbc:postgresql://localhost:5433/bloodbown</url>
<user>postgres</user>
<password>admin</password>
</config>