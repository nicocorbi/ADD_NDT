## FRONT END VS BACK END
aplicacion monolitica- trabajar en el desarollo desde una misma aplicacion o proyecto


### FRONT
parte visual, cercana a nuestro cliente y permite interactuar con la app
ejemplos: pagina web, app movil y usando HTML,CSS y JS


### BACK
parte del servidor que no debe ver el usuario , es la logica de como funciona nuestra aplicacion
trabajamos a partir de una base de datos
PHP,Java,SQL, python


## VENTAJAS DE SEPARAR BACK END CON FRONT END
1. desarrollo independiente -  paralelo sin bloquearse en partes
2. escalabilidad - no depende una de otra
3. facil cambio de tecnologias - no afectan en cambios al estar separados
4. adaptacion mas facil
5. mantenimeiento mas sencillo


## APIS
es como voy a comunicar una parte con otra (back y front)
es un conjjunto de reglas o protocolos para que las apps se comuuniquen entre si
el formato de datos suele ser en JSON


### APIS GRATUITAS
pokeAPI y swapi


### ARQUITECTURA BACKEND
base de datos - contiene la informacion de la app en forma de tablas con filas y columna
clases persistentes - clases de hibernade que represnetan el codigo de nuestras tablas
servicios - clases encargadas de rrealizar operaciones haciendo uso de las clases persistentes
API - permite la comunicacion
 respuestas informativas - empiezan por 1 ( 100, 102 )
 respuestas de errror - empiezan por 4 ( 400, 401, 404 )
 respuestas satisfactorias - empiezan por 2 ( 200, 201 )


 ### OPERACIONES CON APIS
 CRUD
 Crear - POST
 Leer - GET
 Actualizar - PUT
 Eliminar - DELETE
 se basan en HTTP


 ### RUTAS EN UNA API
 utilizamos los metodos POST, PUT, CREATE, DELETE
 recursos : permitir que los clientes hagan las peticiones
 rutas : url que el cliente usa para manipular los recursos se organizan en una estructura de arbol




 ## APUNTES TEMA 8.3 – Implementación de APIs en Java (Spring Boot)
### Controllers
Un controller en Spring Boot es una clase que gestiona las peticiones HTTP de una API.


Se identifica con la anotación:
```java
java
@RestController
Ejemplo:


java
@RestController
public class PeliculaController {
}
```
### RequestMapping
Sirve para indicar qué recurso gestiona un controlador.


Se coloca encima de la clase:
```java
java
@RequestMapping("/peliculas")
Buenas prácticas al nombrar recursos:
```
Usar sustantivos


Minúsculas


Sin caracteres especiales


Separar palabras con “-”


### Servicios en controladores
Los controladores usan servicios para acceder a la base de datos.
```java
Ejemplo:


java
@RestController
@RequestMapping("/peliculas")
public class PeliculaController {


    @Autowired
    private PeliculaService peliculaService;
}
```
### ResponseEntity
Todas las respuestas de una API deben devolver:


Un estado HTTP


Opcionalmente, datos


Ejemplos:
```java
java
return new ResponseEntity<>(id, HttpStatus.CREATED);
return new ResponseEntity<>(HttpStatus.NOT_FOUND);
```
Estados comunes:


OK → Todo correcto


CREATED → Se ha creado un recurso


NOT_FOUND → No existe el recurso


NO_CONTENT → No hay datos que devolver


### Pasar información a la API
Hay dos formas:


A) A través de la URL
Usando @PathVariable o @RequestParam.


Ejemplo:
```java
Código
PUT /peliculas/2?nuevoTitulo=Piratas
java
public ResponseEntity<Void> actualizarPelicula(
    @PathVariable Long id,
    @RequestParam String nuevoTitulo)
```
B) A través del body
Usando @RequestBody.


Ejemplo:


Código
POST /peliculas
Body JSON:
```java
json
{
  "titulo": "Piratas del Caribe 2",
  "director": "Gore Verbinski",
  "duracion": 190
}
```
### Anotación @PathVariable
Se usa cuando queremos trabajar con un recurso concreto:


Obtener por id


Borrar por id


Actualizar por id


Ejemplo:
```java
java
@GetMapping("/{id}")
public ResponseEntity<Pelicula> obtenerPelicula(@PathVariable Long id)
```
### Operación POST
Para insertar un recurso:


Crear método en el controlador


Usar @PostMapping


Recibir datos con @RequestBody


Llamar al servicio para insertar


Ejemplo:
```java
java
@PostMapping("/")
public ResponseEntity<Long> insertarPelicula(@RequestBody Pelicula pelicula)
```
### Operación DELETE
Para borrar un recurso:


Crear método
```java
Usar @DeleteMapping("/{id}")


Recibir id con @PathVariable
```
Ejemplo:


java
```java
@DeleteMapping("/{id}")
public ResponseEntity<Void> borrarPelicula(@PathVariable Long id)
```
### Operación GET sin parámetros
Para obtener todos los recursos:
```java
java
@GetMapping("/")
public ResponseEntity<List<Pelicula>> obtenerPeliculas()
```
### Operación GET con parámetros
Para obtener un recurso concreto:
```java
java
@GetMapping("/{id}")
public ResponseEntity<Pelicula> obtenerPelicula(@PathVariable Long id)
```
También se puede filtrar por atributos:
```java
java
@GetMapping("/duracionMenorA/{minutos}")
public ResponseEntity<List<Pelicula>> obtenerPorDuracion(@PathVariable int minutos)
 ```
### Operación PUT
Para actualizar un recurso:


Crear método


Usar @PutMapping("/{id}")


Recibir id con @PathVariable


Recibir nuevo valor con @RequestParam


Ejemplo:
```java
java
@PutMapping("/{id}")
public ResponseEntity<Void> actualizarPelicula(
    @PathVariable Long id,
    @RequestParam String nuevoTitulo)
 ```
### Postman
Postman sirve para probar APIs:


Elegir método (GET, POST, PUT, DELETE)


Escribir la URL


Añadir parámetros o body


Enviar la petición


Ver la respuesta


Se usa:


Params → para @RequestParam


Body → raw → JSON → para @RequestBody





