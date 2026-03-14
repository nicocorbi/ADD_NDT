## en esta practica hemos usado ECLIPSE
## Autor.java
```java
package practica7.practica7;

import jakarta.persistence.*;
import java.util.ArrayList; 
import java.util.List;
@Entity
@Table(name = "autores")
public class Autor {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;
    private String nacionalidad; 

    public Autor() {}
    public Autor(String nombre, String nacionalidad) {
        this.nombre = nombre;
        this.nacionalidad = nacionalidad;
    }

    // Getters y Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }
    public String getNacionalidad() { return nacionalidad; }
    public void setNacionalidad(String nacionalidad) { this.nacionalidad = nacionalidad; }
}
```
## Libro
```java 
package practica7.practica7;

import jakarta.persistence.*;

@Entity
@Table(name = "libros")
public class Libro {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String titulo;
    private Integer paginas;

    @ManyToOne
    @JoinColumn(name = "autor_id") // Relación: Un libro tiene un autor
    private Autor autor;

    public Libro() {}
    public Libro(String titulo, Integer paginas, Autor autor) {
        this.titulo = titulo;
        this.paginas = paginas;
        this.autor = autor;
    }

    // Getters y Setters
    public Long getId() { return id; }
    public String getTitulo() { return titulo; }
    public void setTitulo(String titulo) { this.titulo = titulo; }
    public Integer getPaginas() { return paginas; }
    public void setPaginas(Integer paginas) { this.paginas = paginas; }
    public Autor getAutor() { return autor; }
    public void setAutor(Autor autor) { this.autor = autor; }
}
```
## AutorController
```java
package practica7.practica7.controllers;

import practica7.practica7.Autor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import java.util.List;

import practica7.practica7.servicios.AutorService;


@RestController
@RequestMapping("/autores")   
public class AutorController {

    @Autowired 
    private AutorService autorService;  
    //  MÉTODO POST → INSERTAR AUTOR
    // URL POSTMAN:  POST http://localhost:8080/autores/
    // Body (JSON):
    // {
    //   "nombre": "Gabriel García Márquez",
    //   "nacionalidad": "Colombiana"
    // }
    @PostMapping("/")
    public ResponseEntity<Long> insertarAutor(@RequestBody Autor autor)
    { 
        // Llama al servicio para insertar el autor en la BD
        Long id = autorService.insertar(autor);

        // Devuelve el ID generado y el código 201 CREATED
        return new ResponseEntity<>(id, HttpStatus.CREATED); 
    }

    //  MÉTODO PUT → ACTUALIZAR  LA NACIONALIDAD
    // URL POSTMAN:  PUT http://localhost:8080/autores/5?nuevaNacionalidad=Argentina
    @PutMapping("/{id}")
    public ResponseEntity<Void> actualizarNacionalidad(
            @PathVariable Long id,                 
            @RequestParam String nuevaNacionalidad 
    ) {

        // Busca el autor por ID
        Autor autor = autorService.obtenerPorId(id);

        // Si no existe, devuelve 404 NOT FOUND
        if (autor == null) {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }

        // Actualiza la nacionalidad
        autorService.actualizarNacionalidad(id, nuevaNacionalidad);

        // Devuelve 200 OK
        return new ResponseEntity<>(HttpStatus.OK);
    }

    // MÉTODO GET → OBTENER AUTOR POR ID
    // URL POSTMAN:  GET http://localhost:8080/autores/3
    @GetMapping("/{id}")
    public ResponseEntity<Autor> obtenerAutor(@PathVariable Long id) {

        // Busca el autor por ID
        Autor autor = autorService.obtenerPorId(id);

        // Si existe, devuelve el autor y 200 OK
        if (autor != null) {
            return new ResponseEntity<>(autor, HttpStatus.OK);
        } 
        // Si no existe, devuelve 404 NOT FOUND
        else {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
    }

    //  MÉTODO GET → OBTENER TODOS LOS AUTORES
    // URL POSTMAN:  GET http://localhost:8080/autores/
    @GetMapping("/")
    public ResponseEntity<List<Autor>> obtenerTodos() {

        // Obtiene la lista completa de autores
        List<Autor> autores = autorService.obtenerTodos();

        // Si la lista está vacía, devuelve 204 NO CONTENT
        if (autores.isEmpty()) {
            return new ResponseEntity<>(HttpStatus.NO_CONTENT);
        }

        // Si hay autores, devuelve la lista y 200 OK
        return new ResponseEntity<>(autores, HttpStatus.OK);
    }

    //  MÉTODO GET → OBTENER AUTORES POR NACIONALIDAD
    // URL POSTMAN:  GET http://localhost:8080/autores/nacionalidad/Española
    @GetMapping("/nacionalidad/{nac}")
    public ResponseEntity<List<Autor>> obtenerPorNacionalidad(@PathVariable String nac) {

        // Busca autores que coincidan con la nacionalidad
        List<Autor> autores = autorService.obtenerPorNacionalidad(nac);

        // Si no hay resultados, devuelve 204 NO CONTENT
        if (autores.isEmpty()) {
            return new ResponseEntity<>(HttpStatus.NO_CONTENT);
        }

        // Si encuentra autores, devuelve lista 
        return new ResponseEntity<>(autores, HttpStatus.OK);
    }

}
```
## LibroController 
```java
package practica7.practica7.controllers;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import java.util.List;

import practica7.practica7.Autor;
import practica7.practica7.Libro;
import practica7.practica7.servicios.AutorService;
import practica7.practica7.servicios.LibroService;

@RestController
@RequestMapping("/libros")   
public class LibroController {
    @Autowired
    private AutorService autorService;  
    @Autowired
    private LibroService libroService;   

    //  MÉTODO POST → INSERTAR LIBRO
    // URL POSTMAN:
    // POST http://localhost:8080/libros?autorId=3
    //
    // Body JSON:
    // {
    //   "titulo": "Cien años de soledad",
    //   "paginas": 471
    // }
    @PostMapping("")
    public ResponseEntity<?> insertarLibro(
            @RequestBody Libro libro,     
            @RequestParam Long autorId) { 

        // Comprobamos si el autor existe
        Autor autor = autorService.obtenerPorId(autorId);

        if (autor == null) {
            return new ResponseEntity<>("No existe un autor con ese ID", HttpStatus.NOT_FOUND);
        }

        // Asociamos el autor al libro
        libro.setAutor(autor);

        // Insertamos el libro y obtenemos su ID
        Long id = libroService.insertar(libro);

        // Devolvemos el ID y código 201 CREATED
        return new ResponseEntity<>(id, HttpStatus.CREATED);
    }

    //  MÉTODO PUT → ACTUALIZAR NÚMERO DE PÁGINAS
    // URL POSTMAN:
    // PUT http://localhost:8080/libros/5/paginas?nuevasPaginas=600
    @PutMapping("/{id}/paginas")
    public ResponseEntity<?> actualizarPaginas(
            @PathVariable Long id,     
            @RequestParam Integer nuevasPaginas) {

        // Comprobamos si el libro existe
        Libro libro = libroService.obtenerPorId(id);

        if (libro == null) {
            return new ResponseEntity<>("No existe un libro con ese ID", HttpStatus.NOT_FOUND);
        }

        // Actualizamos las páginas
        libroService.actualizarPaginas(id, nuevasPaginas);

        // Devolvemos 200 
        return new ResponseEntity<>(HttpStatus.OK);
    }

    //  MÉTODO PUT → ACTUALIZAR LA CLAVE EXTERNA (AUTOR)
    // URL POSTMAN:
    // PUT http://localhost:8080/libros/10/autor/4
    @PutMapping("/{id}/autor/{idAutor}")
    public ResponseEntity<?> actualizarAutorDeLibro(
            @PathVariable Long id,   
            @PathVariable Long idAutor) { 

        // Comprobamos si el libro existe
        Libro libro = libroService.obtenerPorId(id);
        if (libro == null) {
            return new ResponseEntity<>("No existe un libro con ese ID", HttpStatus.NOT_FOUND);
        }

        //  Comprobamos si el autor existe
        Autor autor = autorService.obtenerPorId(idAutor);
        if (autor == null) {
            return new ResponseEntity<>("No existe un autor con ese ID", HttpStatus.NOT_FOUND);
        }

        // Actualizamos la relación libro → autor
        libro.setAutor(autor);
        libroService.actualizar(libro);
        return new ResponseEntity<>(HttpStatus.OK);
    }

    //  MÉTODO DELETE → BORRAR LIBRO POR ID
    // URL POSTMAN:
    // DELETE http://localhost:8080/libros/7
    @DeleteMapping("/{id}")
    public ResponseEntity<?> borrarLibro(@PathVariable Long id) {

        // Comprobamos si el libro existe
        Libro libro = libroService.obtenerPorId(id);

        if (libro == null) {
            return new ResponseEntity<>("No existe un libro con ese ID", HttpStatus.NOT_FOUND);
        }

        // Eliminamos el libro
        libroService.borrar(libro);
        return new ResponseEntity<>(HttpStatus.OK);
    }

    // MÉTODO GET → OBTENER LIBRO POR ID
    // URL POSTMAN:
    // GET http://localhost:8080/libros/2
    @GetMapping("/{id}")
    public ResponseEntity<?> obtenerLibroPorId(@PathVariable Long id) {

        // Buscamos el libro
        Libro libro = libroService.obtenerPorId(id);

        if (libro == null) {
            return new ResponseEntity<>("No existe un libro con ese ID", HttpStatus.NOT_FOUND);
        }

        return new ResponseEntity<>(libro, HttpStatus.OK);
    }

    // MÉTODO GET → OBTENER TODOS LOS LIBROS
    // URL POSTMAN:
    // GET http://localhost:8080/libros
    @GetMapping("")
    public ResponseEntity<?> obtenerTodosLosLibros() {
        List<Libro> libros = libroService.obtenerTodos();     
        if (libros.isEmpty()) {
            return new ResponseEntity<>("No hay libros en la base de datos", HttpStatus.NOT_FOUND);
        }

        //  hay libros, devolvemos lista y 200 
        return new ResponseEntity<>(libros, HttpStatus.OK);
    }

}
```
## AutorService
```java
package practica7.practica7.servicios;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import jakarta.persistence.criteria.CriteriaBuilder;
import jakarta.persistence.criteria.CriteriaQuery;
import jakarta.persistence.criteria.Root;
import practica7.practica7.Autor;

import java.util.List;

@Service   
public class AutorService {

    @Autowired
    private SessionFactory fabricaSesiones;  
    //  INSERTAR AUTOR
    // Inserta un nuevo autor en la base de datos y devuelve su ID
    public Long insertar(Autor nuevoAutor) {
        Session sesion = fabricaSesiones.openSession();  // Abrimos sesión

        try {
            sesion.beginTransaction();   
            sesion.persist(nuevoAutor);  
            sesion.getTransaction().commit();  

            return nuevoAutor.getId();   // Devolvemos el ID generado

        } catch (Exception e) {
            e.printStackTrace();

            // Si algo falla, revertimos la transacción
            if (sesion.getTransaction().isActive()) {
                sesion.getTransaction().rollback();
            }

            return null;  

        } finally {
            sesion.close();  // Cerramos sesión siempre
        }
    }


    //  OBTENER TODOS LOS AUTORES
    // Devuelve una lista con todos los autores de la BD
 
    public List<Autor> obtenerTodos() {
        Session sesion = fabricaSesiones.openSession();

        // Consulta HQL para obtener todos los autores
        List<Autor> lista = sesion.createQuery("FROM Autor", Autor.class).getResultList();

        sesion.close();
        return lista;
    }

    // 3️⃣ BORRAR AUTOR
    // Elimina un autor existente de la base de datos
 
    public void borrar(Autor autorABorrar) {
        Session sesion = fabricaSesiones.openSession();

        try {
            sesion.beginTransaction();     
            sesion.remove(autorABorrar);   
            sesion.getTransaction().commit();  

        } catch (Exception e) {
            e.printStackTrace();

            // Si hay error, revertimos
            if (sesion.getTransaction().isActive()) {
                sesion.getTransaction().rollback();
            }

        } finally {
            sesion.close();  // Cerramos sesión
        }
    }


    // OBTENER AUTOR POR ID
    // Devuelve un autor si existe, o null si no
    public Autor obtenerPorId(Long identificador) {
        Session sesion = fabricaSesiones.openSession();
        Autor autorEncontrado = null;

        try {
            // Busca por clave primaria
            autorEncontrado = sesion.get(Autor.class, identificador);

        } catch (Exception e) {
            e.printStackTrace();

        } finally {
            sesion.close();
        }

        return autorEncontrado;
    }

    // ACTUALIZAR NACIONALIDAD
    // Modifica solo la nacionalidad de un autor
    public void actualizarNacionalidad(Long identificador, String nuevaNacionalidad) {
        Session sesion = fabricaSesiones.openSession();

        try {
            sesion.beginTransaction();

            // Obtenemos el autor a editar
            Autor autorAEditar = sesion.get(Autor.class, identificador);

            if (autorAEditar != null) {
                autorAEditar.setNacionalidad(nuevaNacionalidad);  // Cambiamos el valor
                sesion.merge(autorAEditar);  
            }

            sesion.getTransaction().commit();

        } catch (Exception e) {
            e.printStackTrace();

            if (sesion.getTransaction().isActive()) {
                sesion.getTransaction().rollback();
            }

        } finally {
            sesion.close();
        }
    }


    // 6OBTENER AUTORES POR NACIONALIDAD
    // Devuelve una lista filtrada usando Criteria API

    public List<Autor> obtenerPorNacionalidad(String nacionalidadBuscada) {
        Session session = fabricaSesiones.openSession();
        List<Autor> autores = null;

        try {
            // consulta con Criteria API
            CriteriaBuilder cb = session.getCriteriaBuilder();
            CriteriaQuery<Autor> cq = cb.createQuery(Autor.class);
            Root<Autor> root = cq.from(Autor.class);

            // WHERE nacionalidad = nacionalidadBuscada
            cq.select(root)
              .where(cb.equal(root.get("nacionalidad"), nacionalidadBuscada));

            autores = session.createQuery(cq).getResultList();

        } catch (Exception e) {
            e.printStackTrace();

        } finally {
            session.close();
        }

        return autores;
    }
}
``` 
## LibroService
```java
package practica7.practica7.servicios;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.query.Query;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import practica7.practica7.Libro;
import practica7.practica7.Autor;

import java.util.List;

@Service
public class LibroService {

    @Autowired
    private SessionFactory fabricaSesiones;

    public Long insertar(Libro nuevoLibro) {
        Session sesion = fabricaSesiones.openSession();

        try {
            sesion.beginTransaction();
            sesion.persist(nuevoLibro);
            sesion.getTransaction().commit();
            return nuevoLibro.getId(); 

        } catch (Exception e) {
            e.printStackTrace();
            if (sesion.getTransaction().isActive()) {
                sesion.getTransaction().rollback();
            }
            return null;

        } finally {
            sesion.close();
        }
    }


    public void borrar(Libro libroABorrar) {
        Session sesion = fabricaSesiones.openSession();
        try {
            sesion.beginTransaction();
            Libro libroAdjunto = sesion.merge(libroABorrar);
            sesion.remove(libroAdjunto);
            sesion.getTransaction().commit();
        } catch (Exception e) {
            e.printStackTrace();
            if (sesion.getTransaction().isActive()) {
                sesion.getTransaction().rollback();
            }
        } finally {
            sesion.close();
        }
    }

    public Libro obtenerPorId(Long identificador) {
        Session sesion = fabricaSesiones.openSession();
        Libro libroEncontrado = null;

        try {
            libroEncontrado = sesion.get(Libro.class, identificador);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            sesion.close();
        }

        return libroEncontrado;
    }

    public void actualizarPaginas(Long identificador, Integer nuevasPaginas) {
        Session sesion = fabricaSesiones.openSession();
        try {
            sesion.beginTransaction();

            Libro libroAEditar = sesion.get(Libro.class, identificador);
            if (libroAEditar != null) {
                libroAEditar.setPaginas(nuevasPaginas);
                sesion.merge(libroAEditar);
            }

            sesion.getTransaction().commit();
        } catch (Exception e) {
            e.printStackTrace();
            if (sesion.getTransaction().isActive()) {
                sesion.getTransaction().rollback();
            }
        } finally {
            sesion.close();
        }
    }

    public List<Libro> obtenerPorTitulo(String tituloBuscado) {
        Session sesion = fabricaSesiones.openSession();
        List<Libro> listaDeLibros = null;

        try {
            Query<Libro> consulta = sesion.createQuery(
                "FROM Libro WHERE titulo = :t", 
                Libro.class
            );
            consulta.setParameter("t", tituloBuscado);

            listaDeLibros = consulta.getResultList();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            sesion.close();
        }

        return listaDeLibros;
    }
    public void actualizar(Libro libro) {
        Session sesion = fabricaSesiones.openSession();

        try {
            sesion.beginTransaction();
            sesion.merge(libro);  
            sesion.getTransaction().commit();

        } catch (Exception e) {
            e.printStackTrace();
            if (sesion.getTransaction().isActive()) {
                sesion.getTransaction().rollback();
            }

        } finally {
            sesion.close();
        }
    }


    public List<Libro> obtenerLibrosPorAutor(Autor autorBuscado) {
        Session sesion = fabricaSesiones.openSession();
        List<Libro> listaDeLibros = null;

        try {
            Query<Libro> consulta = sesion.createQuery(
                "FROM Libro WHERE autor = :a", 
                Libro.class
            );
            consulta.setParameter("a", autorBuscado);

            listaDeLibros = consulta.getResultList();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            sesion.close();
        }

        return listaDeLibros;
    }
    public List<Libro> obtenerTodos() {
        Session sesion = fabricaSesiones.openSession();
        List<Libro> libros = null;

        try {
            sesion.beginTransaction();

            libros = sesion.createQuery("from Libro", Libro.class).list();

            sesion.getTransaction().commit();
        } catch (Exception e) {
            e.printStackTrace();
            if (sesion.getTransaction().isActive()) {
                sesion.getTransaction().rollback();
            }
        } finally {
            sesion.close();
        }

        return libros;
    }

}
```
## Practica7Application
```java
package practica7.practica7;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import practica7.practica7.servicios.AutorService;
import practica7.practica7.servicios.LibroService;

import java.util.List;
import java.util.Scanner;

@SpringBootApplication
public class Practica7Application implements CommandLineRunner {

    @Autowired
    private AutorService autorService;

    @Autowired
    private LibroService libroService;

    public static void main(String[] args) {
        SpringApplication.run(Practica7Application.class, args);
    }

    @Override
    public void run(String... args) {
        Scanner lectorTeclado = new Scanner(System.in);
        int opcionSeleccionada = -1;

        while (opcionSeleccionada != 0) {
            System.out.println("\n--- GESTIÓN DE BIBLIOTECA (PRÁCTICA 7) ---");
            System.out.println("1. Añadir Autor");
            System.out.println("2. Borrar Autor");
            System.out.println("3. Buscar Autor por ID");
            System.out.println("4. Actualizar Nacionalidad de Autor");
            System.out.println("5. Añadir Libro (Relacionado con Autor)");
            System.out.println("6. Borrar Libro");
            System.out.println("7. Buscar Libro por ID");
            System.out.println("8. Actualizar Páginas de Libro");
            System.out.println("9. Buscar Libros por Título");
            System.out.println("10. VER TODOS LOS LIBROS DE UN AUTOR");
            System.out.println("0. Salir");
            System.out.print("Elige una opción: ");

            try {
                opcionSeleccionada = Integer.parseInt(lectorTeclado.nextLine());

                switch (opcionSeleccionada) {

                    // 1. Añadir Autor
                    case 1:
                        System.out.print("Nombre del autor: ");
                        String nombreAutor = lectorTeclado.nextLine().trim();

                        if (nombreAutor.isEmpty()) {
                            System.out.println("ERROR: El nombre no puede estar vacío.");
                            break;
                        }

                        System.out.print("Nacionalidad del autor: ");
                        String nacionalidadAutor = lectorTeclado.nextLine().trim();

                        if (nacionalidadAutor.isEmpty()) {
                            System.out.println("ERROR: La nacionalidad no puede estar vacía.");
                            break;
                        }

                        autorService.insertar(new Autor(nombreAutor, nacionalidadAutor));
                        System.out.println("Autor creado correctamente.");
                        break;

                    // 2. Borrar Autor
                    case 2:
                        System.out.print("ID del autor a borrar: ");
                        Long idAutorBorrar = leerLongSeguro(lectorTeclado);
                        if (idAutorBorrar == null) break;

                        Autor autorABorrar = autorService.obtenerPorId(idAutorBorrar);
                        if (autorABorrar == null) {
                            System.out.println("No existe un autor con ese ID.");
                            break;
                        }

                        autorService.borrar(autorABorrar);
                        System.out.println("Autor borrado correctamente.");
                        break;

                    // 3. Buscar Autor por ID
                    case 3:
                        System.out.print("ID del autor a buscar: ");
                        Long idAutorBuscar = leerLongSeguro(lectorTeclado);
                        if (idAutorBuscar == null) break;

                        Autor autorEncontrado = autorService.obtenerPorId(idAutorBuscar);
                        if (autorEncontrado == null) {
                            System.out.println("No existe un autor con ese ID.");
                        } else {
                            System.out.println("Nombre: " + autorEncontrado.getNombre() +
                                               " | Nacionalidad: " + autorEncontrado.getNacionalidad());
                        }
                        break;

                    // 4. Actualizar Nacionalidad
                    case 4:
                        System.out.print("ID del autor para actualizar: ");
                        Long idAutorActualizar = leerLongSeguro(lectorTeclado);
                        if (idAutorActualizar == null) break;

                        Autor autorAEditar = autorService.obtenerPorId(idAutorActualizar);
                        if (autorAEditar == null) {
                            System.out.println("No existe un autor con ese ID.");
                            break;
                        }

                        System.out.print("Nueva nacionalidad: ");
                        String nuevaNacionalidad = lectorTeclado.nextLine().trim();

                        if (nuevaNacionalidad.isEmpty()) {
                            System.out.println("ERROR: La nacionalidad no puede estar vacía.");
                            break;
                        }

                        autorService.actualizarNacionalidad(idAutorActualizar, nuevaNacionalidad);
                        System.out.println("Nacionalidad actualizada.");
                        break;

                    // 5. Añadir Libro
                    case 5:
                        System.out.print("Título del libro: ");
                        String tituloLibro = lectorTeclado.nextLine().trim();

                        if (tituloLibro.isEmpty()) {
                            System.out.println("ERROR: El título no puede estar vacío.");
                            break;
                        }

                        System.out.print("Número de páginas: ");
                        Integer numeroPaginas = leerIntSeguro(lectorTeclado);
                        if (numeroPaginas == null || numeroPaginas <= 0) {
                            System.out.println("ERROR: Las páginas deben ser un número positivo.");
                            break;
                        }

                        System.out.print("ID del Autor: ");
                        Long idAutorParaLibro = leerLongSeguro(lectorTeclado);
                        if (idAutorParaLibro == null) break;

                        Autor autorDelLibro = autorService.obtenerPorId(idAutorParaLibro);
                        if (autorDelLibro == null) {
                            System.out.println("No existe un autor con ese ID.");
                            break;
                        }

                        libroService.insertar(new Libro(tituloLibro, numeroPaginas, autorDelLibro));
                        System.out.println("Libro añadido correctamente.");
                        break;

                    // 6. Borrar Libro
                    case 6:
                        System.out.print("ID del libro a borrar: ");
                        Long idLibroBorrar = leerLongSeguro(lectorTeclado);
                        if (idLibroBorrar == null) break;

                        Libro libroABorrar = libroService.obtenerPorId(idLibroBorrar);
                        if (libroABorrar == null) {
                            System.out.println("No existe un libro con ese ID.");
                            break;
                        }

                        libroService.borrar(libroABorrar);
                        System.out.println("Libro borrado correctamente.");
                        break;

                    // 7. Buscar Libro por ID
                    case 7:
                        System.out.print("ID del libro a buscar: ");
                        Long idLibroBuscar = leerLongSeguro(lectorTeclado);
                        if (idLibroBuscar == null) break;

                        Libro libroEncontrado = libroService.obtenerPorId(idLibroBuscar);
                        if (libroEncontrado == null) {
                            System.out.println("No existe un libro con ese ID.");
                        } else {
                            System.out.println("Título: " + libroEncontrado.getTitulo() +
                                               " | Páginas: " + libroEncontrado.getPaginas());
                        }
                        break;

                    // 8. Actualizar páginas
                    case 8:
                        System.out.print("ID del libro para modificar páginas: ");
                        Long idLibroActualizar = leerLongSeguro(lectorTeclado);
                        if (idLibroActualizar == null) break;

                        Libro libroAEditar = libroService.obtenerPorId(idLibroActualizar);
                        if (libroAEditar == null) {
                            System.out.println("No existe un libro con ese ID.");
                            break;
                        }

                        System.out.print("Nuevas páginas: ");
                        Integer nuevasPaginas = leerIntSeguro(lectorTeclado);
                        if (nuevasPaginas == null || nuevasPaginas <= 0) {
                            System.out.println("ERROR: Las páginas deben ser un número positivo.");
                            break;
                        }

                        libroService.actualizarPaginas(idLibroActualizar, nuevasPaginas);
                        System.out.println("Páginas actualizadas.");
                        break;

                    // 9. Buscar libros por título
                    case 9:
                        System.out.print("Título a buscar: ");
                        String tituloBusqueda = lectorTeclado.nextLine().trim();

                        if (tituloBusqueda.isEmpty()) {
                            System.out.println("ERROR: El título no puede estar vacío.");
                            break;
                        }

                        List<Libro> listaLibros = libroService.obtenerPorTitulo(tituloBusqueda);

                        if (listaLibros.isEmpty()) {
                            System.out.println("No se encontraron libros con ese título.");
                        } else {
                            listaLibros.forEach(libro ->
                                System.out.println("ID: " + libro.getId() +
                                                   " - " + libro.getTitulo() +
                                                   " (" + libro.getPaginas() + " páginas)")
                            );
                        }
                        break;

                    // 10. Ver todos los libros de un autor
                    case 10:
                        System.out.print("ID Autor para listar sus libros: ");
                        Long idAutorListado = leerLongSeguro(lectorTeclado);
                        if (idAutorListado == null) break;

                        Autor autorFiltro = autorService.obtenerPorId(idAutorListado);
                        if (autorFiltro == null) {
                            System.out.println("No existe un autor con ese ID.");
                            break;
                        }

                        List<Libro> librosAutor = libroService.obtenerLibrosPorAutor(autorFiltro);

                        if (librosAutor.isEmpty()) {
                            System.out.println("Este autor no tiene libros registrados.");
                            break;
                        }

                        System.out.println("Libros del autor " + autorFiltro.getNombre() + ":");
                        librosAutor.forEach(libro ->
                            System.out.println(" * " + libro.getTitulo() +
                                               " | Autor: " + libro.getAutor().getNombre() +
                                               " | Páginas: " + libro.getPaginas())
                        );
                        break;

                    case 0:
                        System.out.println("Saliendo...");
                        break;

                    default:
                        System.out.println("Opción no válida.");
                }

            } catch (Exception e) {
                System.out.println("Error inesperado.");
            }
        }

        lectorTeclado.close();
    }

    // MÉTODOS AUXILIARES PARA VALIDAR ENTRADAS

    private Long leerLongSeguro(Scanner sc) {
        try {
            return Long.parseLong(sc.nextLine());
        } catch (NumberFormatException e) {
            System.out.println("ERROR: Debes introducir un número válido.");
            return null;
        }
    }

    private Integer leerIntSeguro(Scanner sc) {
        try {
            return Integer.parseInt(sc.nextLine());
        } catch (NumberFormatException e) {
            System.out.println("ERROR: Debes introducir un número válido.");
            return null;
        }
    }
}
```
