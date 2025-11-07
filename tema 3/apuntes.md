# Trabajo con ficheros XML
## dom y sax
dom- document object model
sax- simple api for xml
leer (sax y dom) y escribit (dom) ficheros xml
sirve para detectar ficheros de xml y verificar si la estructura que esta dando es la esperada

## ventajas y desventajas dom
ventaja- al leer el fichero crea un arbol formado por nodos en memoria
desventaja- almacena todo el fichero en memoria , por ende, es mas lento que sax aunq hace un trabajo mas profundo

## ventajas y desventajas sax
ventaja- lee el fichero linea a linea sin cargar todo el documento en memoria
desventaja- menos potencia que dom al no conocer todo el documento.

dom- para tareas complejas que requieran conocer todo el fichero y tengamos un objetivo claro
sax- para reconocer ficheros secuencialmente y cosas simples

## comparacion
### sax 
basado en eventps (dispara eventos para saber cuando inica y cuando termina una etiqueta xml)
analiza nodo a nodo
analiza sobre la marcha
rapido en tiempo de ejecucion
solo permite lectura de ficheros xml

### dom
carga todo el fichero en memoria
permite buscar tags tanto adelante y hacia atras en todo el documento
almacena el fichero en memoria en forma de arbol
lento en tiempo de ejecucion
se puede insertar o eliminar 

## conversion de ficheros XML
pasers de ficherps XML javax.xml.parents

codigo de ejemplo de pasador dom:
```java
public class EjemploDOM {
    public static void main(String[] args) {
        try {
            // Crear una instancia del parser DOM
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();

            // Cargar el archivo XML
            Document doc = builder.parse(new File("productos.xml"));
            doc.getDocumentElement().normalize();

            // Obtener todos los elementos <producto>
            NodeList lista = doc.getElementsByTagName("producto");

            // Recorrer y mostrar la información
            for (int i = 0; i < lista.getLength(); i++) {
                Node nodo = lista.item(i);

                if (nodo.getNodeType() == Node.ELEMENT_NODE) {
                    Element elemento = (Element) nodo;
                    String nombre = elemento.getElementsByTagName("nombre").item(0).getTextContent();
                    String categoria = elemento.getElementsByTagName("categoria").item(0).getTextContent();
                    String precio = elemento.getElementsByTagName("precio").item(0).getTextContent();

                    System.out.println("Producto: " + nombre);
                    System.out.println("Categoría: " + categoria);
                    System.out.println("Precio: " + precio);
                    System.out.println("-----------------------");
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
```
codigo de paseador de tipo sax:
```java
public class EjemploSAX {
    public static void main(String[] args) {
        try {
            // Crear una fábrica y un parser SAX
            SAXParserFactory factory = SAXParserFactory.newInstance();
            SAXParser saxParser = factory.newSAXParser();

            // Crear un manejador personalizado
            DefaultHandler handler = new DefaultHandler() {
                boolean nombre = false;
                boolean categoria = false;
                boolean precio = false;

                @Override
                public void startElement(String uri, String localName, String qName, Attributes attributes) {
                    if (qName.equalsIgnoreCase("nombre")) nombre = true;
                    if (qName.equalsIgnoreCase("categoria")) categoria = true;
                    if (qName.equalsIgnoreCase("precio")) precio = true;
                }

                @Override
                public void characters(char[] ch, int start, int length) {
                    String contenido = new String(ch, start, length).trim();
                    if (nombre) {
                        System.out.println("Nombre: " + contenido);
                        nombre = false;
                    } else if (categoria) {
                        System.out.println("Categoría: " + contenido);
                        categoria = false;
                    } else if (precio) {
                        System.out.println("Precio: " + contenido);
                        precio = false;
                    }
                }

                @Override
                public void endElement(String uri, String localName, String qName) {
                    if (qName.equalsIgnoreCase("producto")) {
                        System.out.println("-----------------------");
                    }
                }
            };

            // Procesar el XML
            saxParser.parse(new File("productos.xml"), handler);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
