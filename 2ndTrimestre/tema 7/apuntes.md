# EL MAPEO OBJETO RELACIONAL
traducir de un lado a otro con diferente tipo de asociaciones(ejemplo: java base de datos)
clases <--> tabla
objetos <--> filas
atributos <--> columnas

guardar un objeto en base de datos
complejo por las diferencias
nos ayudaremos del "mapeo objeto relacional"(framework)
un ORM o framework nos permite manipular base de datos mediante los objetos de nuestros programas

## vebtajas y desventajas
### ventajas
rapidez en desarrollo
desarrollo orientado a objetos
abstraccion al no usar sentencias
mantenimiento sencillo
### desventajas
mapeo automatico -> peor rendimiento
curva de aprendizaje 
## herramientas ORM
ebean
ibatis
hibernate

### hibernate
tiene una serie de clases y objetos y nos olvidamos de SQL
objetos de persistencia , objetos para manipular la BD por parte de hibernate y permite manipular objetos que se traduciran en sentencias sql
#### .PROPERTIES
archivo de configuracion de hibernate
#### mapeo XML
#### instalacion
spring boot 

## EXPLORANDO EL MAPEO OBJETO RELACIONAL
clases persistentes 
objeto(atributos)<--> base de datos
### reglas a seguir
deben tener un constructor por defecto
deben tener un atributo id que hara de clave primaria 
todos los atributos deben ser privados y tener get y set
### configuracion del proyecto
java --> lenguaje
maven --> gestor de dependencias
hibernate --> orm
spring boot --> framework java
postgreSQL --> base de datos

### configuracion del proyecto
descargar dbeaver y conectamos con la base de datos, para tener una interfaz mas agradable en vez de usar todo en terminal
crear un proyecto de spring boot con una serie de dependencias 
nos descargamos el proyecto y lo abrimos en netbeans
configuramos la conexion a la base de datos
configuramos la ejecucion con el springboot run 

### ejemplos de clases persistentes
las clases persistentes se localizan pq tienen un @ delante 
@Entity indica que es una clase persistente
@Table indica el nombre de referencia de la base de datos
@service --> trabaja con una entidad
@autowired asocia la configuracion del propieties
cerramos la sesion usando finally{
    session.close;
}
### metodos de la clase session
.get(class, long id) - lectura
.delete(Entity entity)
.persist(Entity entity) - almacenamiento
.merge(Entity entity) - actualizacion
## criteriaBuilder,CriteriaQuery y Root
clase que permite consultas mas complejas
creamos el criteriaBuilder
indicamos que vamos a crear una query para la entidad ""
definimos la raiz de la consulta



