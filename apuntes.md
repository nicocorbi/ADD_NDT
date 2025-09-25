# 📘 Tema 1 – Manejo de Ficheros en Java

## 🔹 Introducción
Con el paquete **`java.io`** se pueden realizar operaciones básicas con ficheros y directorios, tales como:
- Crear  
- Modificar  
- Borrar  

Esto permite gestionar información almacenada en el sistema de archivos.

---

## 🔹 Definición de Fichero
Un **fichero** es una sucesión de bits almacenada en un dispositivo.  

### Tipos principales
- **ASCII** → Líneas de texto en formato legible (texto plano).  
- **Binarios** → Contienen información en código binario (imágenes, vídeos, programas, etc.).  

---

## 🔹 Clase `File` (Java.io.File)
La clase **`File`** representa de forma abstracta un fichero o directorio en el disco.  

# ✨ Métodos importantes de `File`

## 1. `createNewFile()`
Crea un fichero nuevo y vacío en la ruta especificada.  
```java
File fichero = new File("/Desktop/file.txt");
fichero.createNewFile();
```
### delete() 
Elimina el fichero o directorio.
fichero.delete();

### mkdir()
Crea un directorio (solo uno, no padres).
```java
File carpeta = new File("/Desktop/carpeta");
carpeta.mkdir();
```

### mkdirs()
Crea directorios y todos sus directorios padres si no existen.
```java
File carpetas = new File("/Desktop/folder/subfolder");
carpetas.mkdirs();
```
### getName()
Devuelve el nombre del fichero o directorio.
fichero.getName();

### renameTo(File dest)
Renombra o mueve un fichero/directorio a otra ubicación.
```java
File fichero1 = new File("/Desktop/file.txt");
File fichero2 = new File("/Desktop/nuevo.txt");
fichero1.renameTo(fichero2);
```
### exists()
Verifica si el fichero o directorio existe.
fichero.exists();
### getPath() y getAbsolutePath()
- getPath() → Devuelve la ruta relativa.
- getAbsolutePath() → Devuelve la ruta absoluta.
fichero.getPath();
fichero.getAbsolutePath();

### listFiles()
```java
File carpeta = new File("/Desktop/folder");
File[] archivos = carpeta.listFiles();
```
## Ejercicios
### Ejercicio 1
Haciendo uso de la clase java.io.File debera crear un directorio con el nombre "cine_granada" dentro de la carpeta "P1" utilizada para la practica
```java
import java.io.File;

public class MyClass {
  public static void main(String args[]) {
    File directory = new File("/P1/cine_granada");
    directory.mkdir();
  }
}
```
### Ejercicio 2
Haciendo uso de la clase Java.io.File debera crear los siguientes directorios dentro de la carpeta "P1"(no confundir con incluir dentro de la carpeta creada en el ejercicio 1): "Lunes,Martes,Miercoles,Jueves,Viernes,etc" 
```java 
import java.io.File;

public class MyClass {
  public static void main(String args[]) {
    String[] dias = {"Lunes", "Martes", "Miércoles", "Jueves", "Viernes", "Sábado", "Domingo"};
    for (String dia : dias) {
        File dir = new File("P1/"+dia);
        dir.mkdir();
    }
  }
}
```
### Ejercicio 3
Haciendo uso de la clase Java.io.File debera mover los siguientes directorios:"Lunes, Martes..." dentro de la carpeta cine_granada
```java
import java.io.File;

public class MyClass {
  public static void main(String args[]) {
    String[] dias = {"Lunes", "Martes", "Miércoles", "Jueves", "Viernes", "Sábado", "Domingo"};
    for (String dia : dias) {
        File origin = new File("P1/"+dia);
        File destination = new File("P1/cine_granada"+ dia);
        origin.renameTo(destination);
    }
  }
}
```
### Ejercicio 4
Hasta este momento hemos creado directorios sin realizar ningun tipo de comprobacion, añada al codigo de los ejercicios 1 y 2 comprobaciones para no crear el directorio si ya existe.
```java
import java.io.File;

public class MyClass {
  public static void main(String args[]) {
    String[] dias = {"Lunes", "Martes", "Miércoles", "Jueves", "Viernes", "Sábado", "Domingo"};
    for (String dia : dias) {
        File dir = new File("P1/"+dia);
        if (!dir.exists()){
            dir.mkdir();
        }
    }
  }
}
```
### Ejercicio 5
A partir de la modificacion realizada en el ejercicio 4 incluya el codigo necesario para imprimir por consola la ruta absoluta de los ficheros o directorios en el momento de crearse. Debera imprimir un mensaje como el siguiente: "Se ha creadi el directorio con la ruta absoluta:/.../.../...".
```java
import java.io.File;

public class MyClass {
  public static void main(String args[]) {
    String[] dias = {"Lunes", "Martes", "Miércoles", "Jueves", "Viernes", "Sábado", "Domingo"};
    for (String dia : dias) {
        File dir = new File("P1/"+dia);
        if (!dir.exists()){
            dir.mkdir();
            System.out.println("Se ha creado el directorio con ruta absoluta:"+ dir.getAbsolutePath());
        }
    }
  }
}
```

