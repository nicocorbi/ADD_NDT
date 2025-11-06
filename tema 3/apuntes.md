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

```
codigo de paseador de tipo sax:
```java

```
## 