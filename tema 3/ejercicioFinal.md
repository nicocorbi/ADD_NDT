# Desarrolle una aplicación en Java que permita leer, consultar y modificar información de un
fichero XML utilizando el método de parseo DOM.
Implemente un programa en Java llamado GestorProductosXML que permita al usuario elegir,
mediante un menú, procesando el fichero XML usando DOM.
## DomManager
```java
package com.nicolas.dom;

import java.io.File;
import java.util.Scanner;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

/**
 *
 * @author Administrator
 */
//TODO: incluir imports necesarios

public class DOMManager {

    private static final String RUTA_XML = "productos.xml";

    // ==========================================================
    // Método 1: Mostrar todos los productos
    // ==========================================================
    public static void mostrarProductos() {
        try {
            Document doc = cargarDocumento();
            NodeList lista = doc.getElementsByTagName("producto"); //PISTA
           for (int i = 0; i < lista.getLength(); i++) {
    
    // Obtener el nodo actual (cada <producto>)
            Node nodo = lista.item(i);

            // Comprobar que el nodo sea un elemento real
            if (nodo.getNodeType() == Node.ELEMENT_NODE) {

                // Convertir el nodo genérico en un Element
                Element elemento = (Element) nodo;

                // Leer los valores de las etiquetas hijas
                String nombre = elemento.getElementsByTagName("nombre").item(0).getTextContent();
                String categoria = elemento.getElementsByTagName("categoria").item(0).getTextContent();
                String precio = elemento.getElementsByTagName("precio").item(0).getTextContent();
                String stock = elemento.getElementsByTagName("stock").item(0).getTextContent();

                // Mostrar los datos por consola
                System.out.println("Nombre: " + nombre);
                System.out.println("Categoría: " + categoria);
                System.out.println("Precio: " + precio);
                System.out.println("Stock: " + stock);
                System.out.println("---------------------------");
            }
        }
          System.out.println("\n=== LISTA DE PRODUCTOS ===");
            //TODO: recorrer el listado de productos y mostrar sus características
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // ==========================================================
    // Método 2: Añadir nuevo producto
    // ==========================================================
  public static void agregarProducto() {
        Scanner sc = new Scanner(System.in);
        try {
            Document doc = cargarDocumento();
            Element raiz = doc.getDocumentElement();

            // 1️⃣ Pedir datos al usuario
            System.out.print("Introduce el ID del producto: ");
            String id = sc.nextLine();// para pedir datos de usuario 

            System.out.print("Introduce el nombre del producto: ");
            String nombre = sc.nextLine();

            System.out.print("Introduce la categoría del producto: ");
            String categoria = sc.nextLine();

            System.out.print("Introduce el precio del producto: ");
            String precio = sc.nextLine();

            System.out.print("Introduce el stock del producto: ");
            String stock = sc.nextLine();

            // 2️⃣ Crear nuevo nodo <producto>
            Element nuevoProducto = doc.createElement("producto");
            nuevoProducto.setAttribute("id", id);

            // crea y añade subnodos 
            Element nombreElem = doc.createElement("nombre");//Crear las etiquetas hijas
            nombreElem.setTextContent(nombre);
            nuevoProducto.appendChild(nombreElem);

            Element categoriaElem = doc.createElement("categoria");
            categoriaElem.setTextContent(categoria);
            nuevoProducto.appendChild(categoriaElem);

            Element precioElem = doc.createElement("precio");
            precioElem.setTextContent(precio);
            nuevoProducto.appendChild(precioElem);

            Element stockElem = doc.createElement("stock");
            stockElem.setTextContent(stock);
            nuevoProducto.appendChild(stockElem);

            //Añado el nuevo producto a la raíz
            raiz.appendChild(nuevoProducto);

            // Guardar los cambios en el XML
            guardarDocumento(doc, RUTA_XML);
            System.out.println(" Producto agregado correctamente.");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }


    // ==========================================================
    // Método 3: Incrementar precios por categoría
    // ==========================================================
public static void incrementarPreciosPorCategoria() {
    Scanner sc = new Scanner(System.in);
    try {
        Document doc = cargarDocumento();
        //TODO: Solicitar la categoría al usuario
        System.out.println("Introduce la categoría a modificar:");
        String categoriaBuscada = sc.nextLine().trim(); // limpio espacios

        boolean encontrado = false; // para saber si se ha modificado algo

        //TODO: Recorrer el listado de productos y modificar su valor
        NodeList lista = doc.getElementsByTagName("producto"); // para obtener lista con todos los nodos de producto.xml
        for (int i = 0; i < lista.getLength(); i++) { // recorrer cada producto
            Node nodo = lista.item(i);
            if (nodo.getNodeType() == Node.ELEMENT_NODE) {
                Element elemento = (Element) nodo;

                String categoria = elemento.getElementsByTagName("categoria").item(0).getTextContent().trim(); // para leer la categoria del producto
                if (categoria.equalsIgnoreCase(categoriaBuscada)) {

                    String precioTexto = elemento.getElementsByTagName("precio").item(0).getTextContent().trim();
                    double precio = Double.parseDouble(precioTexto); // leo el precio actual y luego convierto a número
                    double nuevoPrecio = precio * 1.10;
                    elemento.getElementsByTagName("precio").item(0).setTextContent(String.valueOf(nuevoPrecio)); // para escribir el nuevo precio en el nodo

                    System.out.println("Precio actualizado para producto de categoría: " + categoria);
                    encontrado = true;
                }
            }
        }

        // Guardar los cambios en el archivo productos_actualizados.xml
        guardarDocumento(doc, "productos_actualizados.xml");

        if (encontrado) {
            System.out.println("Precios incrementados un 10% en la categoría: " + categoriaBuscada);
        } else {
            System.out.println("No se encontró ningún producto en la categoría indicada.");
        }

    } catch (Exception e) {
        e.printStackTrace();
    }
}



    // ==========================================================
    // Métodos auxiliares
    // ==========================================================
    private static Document cargarDocumento() throws Exception {
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        DocumentBuilder builder = factory.newDocumentBuilder();
        Document doc = (Document) builder.parse(new File(RUTA_XML));
        doc.getDocumentElement().normalize();
        return doc;
    }

    private static void guardarDocumento(Document doc, String ruta) throws Exception {
        TransformerFactory tf = TransformerFactory.newInstance();
        Transformer transformer = tf.newTransformer();
        transformer.setOutputProperty(OutputKeys.INDENT, "yes");

        DOMSource source = new DOMSource((Node) doc);
        StreamResult result = new StreamResult(new File(ruta));

        transformer.transform(source, result);
    }
}
```
## Gestor productos XML
```java
package com.nicolas.dom;

import com.nicolas.dom.DOMManager;
import java.util.Scanner;

/**
 *
 * @author Administrator
 */
public class GestorProductosXML {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int opcion;

        do {
            mostrarMenu();
            opcion = Integer.parseInt(sc.nextLine());

            switch (opcion) {
                case 1 -> DOMManager.mostrarProductos();
                case 2 -> DOMManager.agregarProducto();
                case 3 -> DOMManager.incrementarPreciosPorCategoria();
                case 0 -> System.out.println("Saliendo del programa...");
                default -> System.out.println("Opción incorrecta. Intente de nuevo.");
            }

            System.out.println();
        } while (opcion != 0);

        sc.close();
    }

    private static void mostrarMenu() {
        System.out.println("===== GESTOR DE PRODUCTOS XML =====");
        System.out.println("1. Mostrar todos los productos (DOM)");
        System.out.println("2. Añadir un nuevo producto (DOM)");
        System.out.println("3. Incrementar precios de una categoría (DOM)");
        System.out.println("4. Generar informe de stock por categoría (SAX)");
        System.out.println("5. Mostrar precio medio global (SAX)");
        System.out.println("0. Salir");
        System.out.print("Seleccione una opción: ");
    }
}
```
// productos.xml
<productos>
    <producto id="P1">
        <nombre>Portátil Lenovo</nombre>
        <categoria>INF</categoria>
        <precio>799.99</precio>
        <stock>15</stock>
    </producto>
    <producto id="P2">
        <nombre>Ratón Logitech</nombre>
        <categoria>PER</categoria>
        <precio>25.50</precio>
        <stock>50</stock>
    </producto>
    <producto id="P3">
        <nombre>Monitor Samsung</nombre>
        <categoria>INF</categoria>
        <precio>199.99</precio>
        <stock>30</stock>
    </producto>
    <producto id="P4">
        <nombre>Teclado Razer</nombre>
        <categoria>GAM</categoria>
        <precio>99.99</precio>
        <stock>10</stock>
    </producto>
</productos>