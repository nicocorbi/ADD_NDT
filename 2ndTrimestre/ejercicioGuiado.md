# Tutorial paso a paso: Aplicación Java Swing con base de datos H2

Este tutorial explica **paso a paso** cómo crear una aplicación Java de escritorio usando **Swing**, **CardLayout** y una **base de datos H2**, tal y como la que has desarrollado. Está pensada para que la copies directamente en tus **apuntes de Visual Studio / VS Code**.

---

## 1. Creación del proyecto

1. Abre tu IDE (NetBeans o VS Code con Java).
2. Crea un **proyecto Java**.
3. Asigna un nombre al proyecto (por ejemplo: `GestorUsuarios`).
4. Usa una estructura de paquetes como:

   ```
   com.example.nico.demo
   ```

---

## 2. Base de datos H2

### 2.1 Crear la base de datos

Se utiliza una base de datos **H2 embebida**, almacenada en archivo:

```java
jdbc:h2:file:/data/practica5db
```

Credenciales:

* Usuario: `sa`
* Contraseña: `password`

---

### 2.2 Tablas utilizadas

```sql
USUARIOS(id, nombre)
PRODUCTOS(id, nombre, precio)
PEDIDOS(id, id_usuario, id_producto, cantidad)
```

---

## 3. Uso de CardLayout

Para navegar entre pantallas se usa `CardLayout`.

### 3.1 Crear contenedor principal

```java
JPanel contenedor = new JPanel();
CardLayout cardLayout = new CardLayout();
contenedor.setLayout(cardLayout);
```

### 3.2 Añadir paneles

```java
contenedor.add(menuPrincipal, "menuPrincipal");
contenedor.add(añadirUsuario, "añadirUsuario");
contenedor.add(añadirProducto, "añadirProducto");
contenedor.add(añadirPedido, "añadirPedido");
```

### 3.3 Cambiar de pantalla

```java
cardLayout.show(contenedor, "añadirUsuario");
```

---

## 4. Panel MenuPrincipal

### Función

Pantalla principal desde la que se accede a:

* Añadir usuario
* Añadir producto
* Añadir pedido
* Mostrar tabla de pedidos

### Elementos

* Botones (`JButton`)
* Título (`JLabel`)
* Navegación con `CardLayout`

---

## 5. Panel AñadirUsuario

### Función

Insertar un nuevo usuario en la base de datos.

### Proceso

1. Leer nombre del `TextField`
2. Validar que no esté vacío
3. Conectar a H2
4. Ejecutar `INSERT`

```java
INSERT INTO usuarios (nombre) VALUES (?);
```

---

## 6. Panel AñadirProducto

### Función

Insertar un nuevo producto.

### Proceso

1. Leer nombre y precio
2. Convertir precio a `double`
3. Insertar en la tabla productos

```java
INSERT INTO productos (nombre, precio) VALUES (?, ?);
```

---

## 7. Panel AñadirPedido

### Función

Insertar un pedido relacionando usuario y producto.

### Proceso

1. Obtener IDs desde `JSpinner`
2. Validar cantidad
3. Insertar pedido

```java
INSERT INTO pedidos (id_usuario, id_producto, cantidad)
VALUES (?, ?, ?);
```

---

## 8. Mostrar datos en JTable

### Objetivo

Mostrar los pedidos en una tabla.

### Pasos

1. Ejecutar `SELECT * FROM pedidos`
2. Obtener metadatos
3. Crear `DefaultTableModel`
4. Añadir filas dinámicamente
5. Mostrar en `JTable`

---

## 9. Buenas prácticas aplicadas

* Uso de `PreparedStatement`
* Separación de pantallas
* Validaciones básicas
* Cierre de conexiones
* Código limpio y estructurado

---

## 10. Resultado final

La aplicación permite:

* Gestionar usuarios
* Gestionar productos
* Crear pedidos
* Visualizar pedidos

Todo ello usando **Java Swing**, **H2** y **CardLayout**.

---

