## DBConection
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DBConnection {

    private static final String URL = "http://localhost:8080/h2-console";
    private static final String USER = "sa";
    private static final String PASS = "password";

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(URL, USER, PASS);
    }
}
```
## Añadir pedido
```java
// Paquete de la aplicación
package com.example.nico.demo;

// Imports necesarios para interfaz gráfica y base de datos
import java.awt.CardLayout;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import javax.swing.JPanel;

/**
 * Panel para añadir pedidos
 */
public class AñadirPedido extends javax.swing.JPanel {

    // Panel contenedor principal
    private JPanel contenedor;
    // Layout para cambiar de pantallas
    private CardLayout cardLayout;

    /**
     * Constructor vacío
     */
    public AñadirPedido() {
        initComponents();
    }

    /**
     * Constructor con CardLayout
     */
    public AñadirPedido(JPanel contenedor, CardLayout cardLayout) {
        this.contenedor = contenedor;
        this.cardLayout = cardLayout;
        initComponents();
    }

    /**
     * Inicializa los componentes gráficos
     */
    @SuppressWarnings("unchecked")
    private void initComponents() {

        // Inicialización de componentes
        jLabel1 = new javax.swing.JLabel();
        jLabel2 = new javax.swing.JLabel();
        jSpinner1 = new javax.swing.JSpinner();
        jLabel3 = new javax.swing.JLabel();
        jSpinner2 = new javax.swing.JSpinner();
        textField2 = new java.awt.TextField();
        jLabel4 = new javax.swing.JLabel();
        jButton4 = new javax.swing.JButton();
        jButton3 = new javax.swing.JButton();

        // Título
        jLabel1.setText("AÑADIR PEDIDO");

        // Etiqueta usuario
        jLabel2.setText("Usuario");

        // Etiqueta producto
        jLabel3.setText("Producto");

        // Campo de texto cantidad
        textField2.setText("textField1");

        // Etiqueta cantidad
        jLabel4.setText("Cantidad");

        // Botón añadir pedido
        jButton4.setText("añadir producto");
        jButton4.setActionCommand("añadir pedido");
        jButton4.addActionListener(evt -> jButton4ActionPerformed(evt));

        // Botón atrás
        jButton3.setText("Atras");
        jButton3.addActionListener(evt -> jButton3ActionPerformed(evt));

        // Layout del panel
        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(this);
        this.setLayout(layout);

        // Configuración horizontal
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addGroup(layout.createSequentialGroup()
                        .addGap(20)
                        .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                            .addComponent(jLabel2)
                            .addComponent(jSpinner1, javax.swing.GroupLayout.PREFERRED_SIZE, 330, javax.swing.GroupLayout.PREFERRED_SIZE)
                            .addComponent(jLabel3)
                            .addComponent(jSpinner2, javax.swing.GroupLayout.PREFERRED_SIZE, 330, javax.swing.GroupLayout.PREFERRED_SIZE)
                            .addComponent(jLabel4)
                            .addComponent(textField2, javax.swing.GroupLayout.PREFERRED_SIZE, 312, javax.swing.GroupLayout.PREFERRED_SIZE)
                            .addComponent(jButton4, javax.swing.GroupLayout.PREFERRED_SIZE, 151, javax.swing.GroupLayout.PREFERRED_SIZE)))
                    .addGroup(layout.createSequentialGroup()
                        .addGap(175)
                        .addComponent(jLabel1)))
                .addContainerGap(102, Short.MAX_VALUE))
            .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                .addGap(0, 0, Short.MAX_VALUE)
                .addComponent(jButton3, javax.swing.GroupLayout.PREFERRED_SIZE, 67, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addGap(14))
        );

        // Configuración vertical
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addGap(20)
                .addComponent(jLabel1)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addComponent(jLabel2)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addComponent(jSpinner1, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addComponent(jLabel3)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addComponent(jSpinner2, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addComponent(jLabel4)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addComponent(textField2, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addComponent(jButton4, javax.swing.GroupLayout.PREFERRED_SIZE, 35, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.UNRELATED)
                .addComponent(jButton3, javax.swing.GroupLayout.PREFERRED_SIZE, 35, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addContainerGap(12, Short.MAX_VALUE))
        );
    }

    /**
     * Acción del botón añadir pedido
     */
    private void jButton4ActionPerformed(java.awt.event.ActionEvent evt) {

        // Obtener valores de los campos
        int usuarioId = (int) jSpinner1.getValue();
        int productoId = (int) jSpinner2.getValue();
        String cantidadTexto = textField2.getText();

        // Validar cantidad vacía
        if (cantidadTexto.equals("")) {
            System.out.println("La cantidad no puede estar vacía");
            return;
        }

        int cantidad = Integer.parseInt(cantidadTexto);

        // Validar cantidad positiva
        if (cantidad <= 0) {
            System.out.println("La cantidad debe ser mayor que 0");
            return;
        }

        // Datos de conexión a la base de datos
        String URL_CONEXION = "jdbc:h2:file:/data/practica5db";
        String usuario = "sa";
        String password = "password";

        try {
            // Conexión a la base de datos
            Connection conn = DriverManager.getConnection(URL_CONEXION, usuario, password);

            // Consulta SQL
            String sql = "INSERT INTO pedidos (id_usuario, id_producto, cantidad) VALUES (?, ?, ?)";
            PreparedStatement pstmt = conn.prepareStatement(sql);

            // Asignar valores
            pstmt.setInt(1, usuarioId);
            pstmt.setInt(2, productoId);
            pstmt.setInt(3, cantidad);

            // Ejecutar inserción
            int filas = pstmt.executeUpdate();

            // Comprobar resultado
            if (filas > 0) {
                System.out.println("Pedido insertado correctamente");
            } else {
                System.out.println("No se pudo insertar el pedido");
            }

            // Cerrar conexión
            conn.close();

        } catch (Exception ex) {
            System.out.println("Error al insertar pedido: " + ex);
            ex.printStackTrace();
        }
    }

    /**
     * Acción del botón atrás
     */
    private void jButton3ActionPerformed(java.awt.event.ActionEvent evt) {
        cardLayout.show(contenedor, "menuPrincipal");
    }

    /**
     * Método main para pruebas
     */
    public static void main(String[] args) {
        javax.swing.SwingUtilities.invokeLater(() -> {
            javax.swing.JFrame frame = new javax.swing.JFrame("Probar Añadir Pedido");
            frame.setDefaultCloseOperation(javax.swing.JFrame.EXIT_ON_CLOSE);
            frame.setContentPane(new AñadirPedido());
            frame.pack();
            frame.setLocationRelativeTo(null);
            frame.setVisible(true);
        });
    }

    // Declaración de variables
    private javax.swing.JButton jButton3;
    private javax.swing.JButton jButton4;
    private javax.swing.JLabel jLabel1;
    private javax.swing.JLabel jLabel2;
    private javax.swing.JLabel jLabel3;
    private javax.swing.JLabel jLabel4;
    private javax.swing.JSpinner jSpinner1;
    private javax.swing.JSpinner jSpinner2;
    private java.awt.TextField textField2;
}

```

## añadir usuario
```java
/*
 * Plantilla de licencia de NetBeans
 */
package com.example.nico.demo;

// Imports para layout, base de datos y Swing
import java.awt.CardLayout;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import javax.swing.JPanel;

/**
 * Panel para añadir usuarios
 */
public class AñadirUsuario extends javax.swing.JPanel {

    // Panel contenedor principal
    private JPanel contenedor;
    // Layout para cambiar de pantallas
    private CardLayout cardLayout;

    /**
     * Constructor vacío
     */
    public AñadirUsuario() {
        initComponents();
    }

    /**
     * Constructor con CardLayout
     */
    public AñadirUsuario(JPanel contenedor, CardLayout cardLayout) {
        this.contenedor = contenedor;
        this.cardLayout = cardLayout;
        initComponents();
    }

    /**
     * Inicializa los componentes gráficos
     */
    @SuppressWarnings("unchecked")
    private void initComponents() {

        // Inicialización de componentes
        jLabel2 = new javax.swing.JLabel();
        jLabel1 = new javax.swing.JLabel();
        textField1 = new java.awt.TextField();
        jButton4 = new javax.swing.JButton();
        jButton3 = new javax.swing.JButton();

        // Etiqueta nombre de usuario
        jLabel2.setText("Nombre de usuario");

        // Título
        jLabel1.setText("AÑADIR USUARIO");

        // Campo de texto nombre
        textField1.setText("textField1");

        // Botón añadir usuario
        jButton4.setText("añadir usuario");
        jButton4.addActionListener(evt -> jButton4ActionPerformed(evt));

        // Botón atrás
        jButton3.setText("Atras");
        jButton3.addActionListener(evt -> jButton3ActionPerformed(evt));

        // Layout del panel
        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(this);
        this.setLayout(layout);

        // Configuración horizontal
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addContainerGap()
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addComponent(jButton4, javax.swing.GroupLayout.PREFERRED_SIZE, 151, javax.swing.GroupLayout.PREFERRED_SIZE)
                    .addComponent(jLabel2)
                    .addComponent(textField1, javax.swing.GroupLayout.PREFERRED_SIZE, 312, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addContainerGap(78, Short.MAX_VALUE))
            .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                .addGap(0, 0, Short.MAX_VALUE)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addComponent(jLabel1)
                    .addComponent(jButton3, javax.swing.GroupLayout.PREFERRED_SIZE, 67, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addContainerGap())
        );

        // Configuración vertical
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addGap(26)
                .addComponent(jLabel1)
                .addGap(30)
                .addComponent(jLabel2)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addComponent(textField1, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addGap(22)
                .addComponent(jButton4, javax.swing.GroupLayout.PREFERRED_SIZE, 35, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED, 84, Short.MAX_VALUE)
                .addComponent(jButton3, javax.swing.GroupLayout.PREFERRED_SIZE, 35, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addContainerGap())
        );
    }

    /**
     * Acción del botón añadir usuario
     */
    private void jButton4ActionPerformed(java.awt.event.ActionEvent evt) {

        // Obtener nombre del usuario
        String nombre = textField1.getText();

        // Comprobar campo no vacío
        if (!nombre.equals("")) {

            // Datos de conexión
            String URL_CONEXION = "jdbc:h2:file:/data/practica5db";
            String usuario = "sa";
            String password = "password";

            try {
                // Conexión a la base de datos
                Connection conn = DriverManager.getConnection(
                        URL_CONEXION, usuario, password);

                // Consulta SQL
                String sql = "INSERT INTO usuarios (nombre) VALUES (?)";
                PreparedStatement pstmt = conn.prepareStatement(sql);

                // Asignar valor
                pstmt.setString(1, nombre);

                // Ejecutar inserción
                int filas = pstmt.executeUpdate();

                // Comprobar resultado
                if (filas > 0) {
                    System.out.println("Se insertó correctamente");
                } else {
                    System.out.println("No se pudo insertar");
                }

                // Cerrar conexión
                conn.close();

            } catch (Exception ex) {
                System.out.println("Error: " + ex);
                ex.printStackTrace();
            }
        }
    }

    /**
     * Acción del botón atrás
     */
    private void jButton3ActionPerformed(java.awt.event.ActionEvent evt) {
        cardLayout.show(contenedor, "menuPrincipal");
    }

    /**
     * Método main para pruebas
     */
    public static void main(String args[]) {

        // Configurar Look and Feel Nimbus
        try {
            for (javax.swing.UIManager.LookAndFeelInfo info :
                    javax.swing.UIManager.getInstalledLookAndFeels()) {
                if ("Nimbus".equals(info.getName())) {
                    javax.swing.UIManager.setLookAndFeel(info.getClassName());
                    break;
                }
            }
        } catch (Exception ex) {
            // Ignorar errores de Look and Feel
        }

        // Mostrar el panel
        java.awt.EventQueue.invokeLater(() ->
                new AñadirUsuario().setVisible(true));
    }

    // Declaración de variables
    private javax.swing.JButton jButton3;
    private javax.swing.JButton jButton4;
    private javax.swing.JLabel jLabel1;
    private javax.swing.JLabel jLabel2;
    private java.awt.TextField textField1;
}

```

## añadir producto
```java
/*
 * Plantilla de licencia de NetBeans
 */
package com.example.nico.demo;

// Imports para layout, base de datos y Swing
import java.awt.CardLayout;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import javax.swing.JPanel;

/**
 * Panel para añadir productos
 */
public class AñadirProducto extends javax.swing.JPanel {

    // Panel contenedor principal
    private JPanel contenedor;
    // Layout para cambiar de pantallas
    private CardLayout cardLayout;

    /**
     * Constructor vacío
     */
    public AñadirProducto() {
        initComponents();
    }

    /**
     * Constructor con CardLayout
     */
    public AñadirProducto(JPanel contenedor, CardLayout cardLayout) {
        this.contenedor = contenedor;
        this.cardLayout = cardLayout;
        initComponents();
    }

    /**
     * Inicializa los componentes gráficos
     */
    @SuppressWarnings("unchecked")
    private void initComponents() {

        // Inicialización de componentes
        jLabel2 = new javax.swing.JLabel();
        textField1 = new java.awt.TextField();
        jButton4 = new javax.swing.JButton();
        jButton3 = new javax.swing.JButton();
        jLabel3 = new javax.swing.JLabel();
        textField2 = new java.awt.TextField();

        // Etiqueta nombre del producto
        jLabel2.setText("Nombre del producto");

        // Campo de texto nombre
        textField1.setText("textField1");

        // Botón añadir producto
        jButton4.setText("añadir producto");
        jButton4.addActionListener(evt -> jButton4ActionPerformed(evt));

        // Botón atrás
        jButton3.setText("Atras");
        jButton3.addActionListener(evt -> jButton3ActionPerformed(evt));

        // Etiqueta precio del producto
        jLabel3.setText("Precio del producto");

        // Campo de texto precio
        textField2.setText("textField1");

        // Layout del panel
        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(this);
        this.setLayout(layout);

        // Configuración horizontal
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addContainerGap()
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addComponent(jButton4, javax.swing.GroupLayout.PREFERRED_SIZE, 151, javax.swing.GroupLayout.PREFERRED_SIZE)
                    .addComponent(jLabel2)
                    .addComponent(textField1, javax.swing.GroupLayout.PREFERRED_SIZE, 312, javax.swing.GroupLayout.PREFERRED_SIZE)
                    .addComponent(jLabel3)
                    .addComponent(textField2, javax.swing.GroupLayout.PREFERRED_SIZE, 312, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addContainerGap(78, Short.MAX_VALUE))
            .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                .addContainerGap(javax.swing.GroupLayout.DEFAULT_SIZE, Short.MAX_VALUE)
                .addComponent(jButton3, javax.swing.GroupLayout.PREFERRED_SIZE, 67, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addGap(14))
        );

        // Configuración vertical
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addGap(36)
                .addComponent(jLabel2)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addComponent(textField1, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addComponent(jLabel3)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addComponent(textField2, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addGap(18)
                .addComponent(jButton4, javax.swing.GroupLayout.PREFERRED_SIZE, 35, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED, 59, Short.MAX_VALUE)
                .addComponent(jButton3, javax.swing.GroupLayout.PREFERRED_SIZE, 35, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addGap(15))
        );
    }

    /**
     * Acción del botón añadir producto
     */
    private void jButton4ActionPerformed(java.awt.event.ActionEvent evt) {

        // Obtener datos del formulario
        String nombre = textField1.getText();
        String precioTxt = textField2.getText();

        // Comprobar campos no vacíos
        if (!nombre.equals("") && !precioTxt.equals("")) {

            // Datos de conexión
            String URL_CONEXION = "jdbc:h2:file:/data/practica5db";
            String usuario = "sa";
            String password = "password";

            try {
                // Convertir precio a double
                double precio = Double.parseDouble(precioTxt);

                // Conexión a la base de datos
                Connection conn = DriverManager.getConnection(
                        URL_CONEXION, usuario, password);

                // Consulta SQL
                String sql = "INSERT INTO productos (nombre, precio) VALUES (?, ?)";
                PreparedStatement pstmt = conn.prepareStatement(sql);

                // Asignar valores
                pstmt.setString(1, nombre);
                pstmt.setDouble(2, precio);

                // Ejecutar inserción
                int filas = pstmt.executeUpdate();

                // Comprobar resultado
                if (filas > 0) {
                    System.out.println("Producto insertado correctamente");
                }

                // Cerrar conexión
                conn.close();

            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }
    }

    /**
     * Acción del botón atrás
     */
    private void jButton3ActionPerformed(java.awt.event.ActionEvent evt) {
        cardLayout.show(contenedor, "menuPrincipal");
    }

    /**
     * Método main para pruebas
     */
    public static void main(String[] args) {
        javax.swing.SwingUtilities.invokeLater(() -> {
            javax.swing.JFrame frame = new javax.swing.JFrame("Añadir producto");
            frame.setDefaultCloseOperation(javax.swing.JFrame.EXIT_ON_CLOSE);
            frame.setContentPane(new AñadirProducto());
            frame.pack();
            frame.setLocationRelativeTo(null);
            frame.setVisible(true);
        });
    }

    // Declaración de variables
    private javax.swing.JButton jButton3;
    private javax.swing.JButton jButton4;
    private javax.swing.JLabel jLabel2;
    private javax.swing.JLabel jLabel3;
    private java.awt.TextField textField1;
    private java.awt.TextField textField2;
}

```
## menu principal
```java
/*
 * Plantilla de licencia de NetBeans
 */
package com.example.nico.demo;

// Imports para layout, base de datos y Swing
import java.awt.CardLayout;
import java.sql.Connection;
import java.sql.DriverManager;
import javax.swing.JFrame;
import javax.swing.JPanel;

/**
 * Panel del menú principal
 */
public class MenuPrincipal extends javax.swing.JPanel {

    // Panel contenedor principal
    private JPanel contenedor;
    // Layout para cambiar de pantallas
    private CardLayout cardLayout;

    /**
     * Constructor vacío
     */
    public MenuPrincipal() {
        initComponents();
    }

    /**
     * Constructor con CardLayout
     */
    public MenuPrincipal(JPanel contenedor, CardLayout cardLayout) {
        this.contenedor = contenedor;
        this.cardLayout = cardLayout;
        initComponents();
    }

    /**
     * Inicializa los componentes gráficos
     */
    @SuppressWarnings("unchecked")
    private void initComponents() {

        // Barra de menú
        jMenuBar1 = new javax.swing.JMenuBar();
        jMenu1 = new javax.swing.JMenu();
        jMenu2 = new javax.swing.JMenu();

        // Botones del menú
        jButton1 = new javax.swing.JButton();
        jButton3 = new javax.swing.JButton();
        jButton4 = new javax.swing.JButton();
        jButton2 = new javax.swing.JButton();

        // Título
        jLabel1 = new javax.swing.JLabel();

        // Menús
        jMenu1.setText("File");
        jMenuBar1.add(jMenu1);

        jMenu2.setText("Edit");
        jMenuBar1.add(jMenu2);

        // Botón añadir pedido
        jButton1.setText("añadir pedido");
        jButton1.addActionListener(evt -> jButton1ActionPerformed(evt));

        // Botón añadir usuario
        jButton3.setText("añadir usuario");
        jButton3.addActionListener(evt -> jButton3ActionPerformed(evt));

        // Botón añadir producto
        jButton4.setText("añadir producto");
        jButton4.addActionListener(evt -> jButton4ActionPerformed(evt));

        // Título del panel
        jLabel1.setText("Gestor de Usuarios");

        // Botón mostrar pedidos
        jButton2.setText("tablas creadas");
        jButton2.addActionListener(evt -> jButton2ActionPerformed(evt));

        // Layout del panel
        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(this);
        this.setLayout(layout);

        // Configuración horizontal
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addGroup(layout.createSequentialGroup()
                        .addGap(122)
                        .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                            .addComponent(jButton1, javax.swing.GroupLayout.PREFERRED_SIZE, 151, javax.swing.GroupLayout.PREFERRED_SIZE)
                            .addComponent(jButton4, javax.swing.GroupLayout.PREFERRED_SIZE, 151, javax.swing.GroupLayout.PREFERRED_SIZE)
                            .addComponent(jButton3, javax.swing.GroupLayout.PREFERRED_SIZE, 151, javax.swing.GroupLayout.PREFERRED_SIZE)
                            .addComponent(jButton2, javax.swing.GroupLayout.PREFERRED_SIZE, 151, javax.swing.GroupLayout.PREFERRED_SIZE)))
                    .addGroup(layout.createSequentialGroup()
                        .addGap(147)
                        .addComponent(jLabel1)))
                .addContainerGap(127, Short.MAX_VALUE))
        );

        // Configuración vertical
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                .addContainerGap()
                .addComponent(jLabel1, javax.swing.GroupLayout.PREFERRED_SIZE, 36, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED, 41, Short.MAX_VALUE)
                .addComponent(jButton3, javax.swing.GroupLayout.PREFERRED_SIZE, 35, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addGap(18)
                .addComponent(jButton4, javax.swing.GroupLayout.PREFERRED_SIZE, 35, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addGap(18)
                .addComponent(jButton1, javax.swing.GroupLayout.PREFERRED_SIZE, 35, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addGap(18)
                .addComponent(jButton2, javax.swing.GroupLayout.PREFERRED_SIZE, 35, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addGap(41))
        );
    }

    /**
     * Acción botón añadir pedido
     */
    private void jButton1ActionPerformed(java.awt.event.ActionEvent evt) {
        cardLayout.show(contenedor, "añadirUsuario");
    }

    /**
     * Acción botón añadir usuario
     */
    private void jButton3ActionPerformed(java.awt.event.ActionEvent evt) {
        cardLayout.show(contenedor, "añadirUsuario");
    }

    /**
     * Acción botón añadir producto
     */
    private void jButton4ActionPerformed(java.awt.event.ActionEvent evt) {
        cardLayout.show(contenedor, "añadirProducto");
    }

    /**
     * Acción botón mostrar tabla de pedidos
     */
    private void jButton2ActionPerformed(java.awt.event.ActionEvent evt) {

        // Crear ventana para mostrar pedidos
        JFrame pedidosFrame = new JFrame("Pedidos Realizados");
        pedidosFrame.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
        pedidosFrame.setSize(600, 400);
        pedidosFrame.setLocationRelativeTo(null);

        // Panel con BorderLayout
        JPanel panel = new JPanel(new java.awt.BorderLayout());

        try {
            // Datos de conexión
            String URL_CONEXION = "jdbc:h2:file:/data/practica5db";
            String usuario = "sa";
            String password = "password";

            // Conexión a la base de datos
            Connection conn = DriverManager.getConnection(URL_CONEXION, usuario, password);
            java.sql.Statement stmt = conn.createStatement();
            java.sql.ResultSet rs = stmt.executeQuery("SELECT * FROM pedidos");

            // Modelo de la tabla
            javax.swing.table.DefaultTableModel model = new javax.swing.table.DefaultTableModel();
            java.sql.ResultSetMetaData meta = rs.getMetaData();
            int colCount = meta.getColumnCount();

            // Añadir columnas
            for (int i = 1; i <= colCount; i++) {
                model.addColumn(meta.getColumnName(i));
            }

            // Añadir filas
            while (rs.next()) {
                Object[] row = new Object[colCount];
                for (int i = 0; i < colCount; i++) {
                    row[i] = rs.getObject(i + 1);
                }
                model.addRow(row);
            }

            // Crear tabla
            javax.swing.JTable table = new javax.swing.JTable(model);
            panel.add(new javax.swing.JScrollPane(table), java.awt.BorderLayout.CENTER);

            // Cerrar recursos
            rs.close();
            stmt.close();
            conn.close();

        } catch (Exception ex) {
            ex.printStackTrace();
        }

        // Mostrar ventana
        pedidosFrame.setContentPane(panel);
        pedidosFrame.setVisible(true);
    }

    /**
     * Método main de la aplicación
     */
    public static void main(String[] args) {

        java.awt.EventQueue.invokeLater(() -> {

            // Ventana principal
            JFrame frame = new JFrame("Gestor de Usuarios");
            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

            // Contenedor con CardLayout
            JPanel contenedor = new JPanel();
            CardLayout cardLayout = new CardLayout();
            contenedor.setLayout(cardLayout);

            // Crear paneles
            MenuPrincipal menuPrincipal = new MenuPrincipal(contenedor, cardLayout);
            AñadirUsuario añadirUsuario = new AñadirUsuario(contenedor, cardLayout);
            AñadirProducto añadirProducto = new AñadirProducto(contenedor, cardLayout);
            AñadirPedido añadirPedido = new AñadirPedido(contenedor, cardLayout);

            // Añadir paneles al contenedor
            contenedor.add(menuPrincipal, "menuPrincipal");
            contenedor.add(añadirUsuario, "añadirUsuario");
            contenedor.add(añadirProducto, "añadirProducto");
            contenedor.add(añadirPedido, "añadirPedido");

            // Mostrar ventana
            frame.setContentPane(contenedor);
            frame.pack();
            frame.setLocationRelativeTo(null);
            frame.setVisible(true);

            // Mostrar menú principal
            cardLayout.show(contenedor, "menuPrincipal");
        });
    }

    // Declaración de variables
    private javax.swing.JButton jButton1;
    private javax.swing.JButton jButton2;
    private javax.swing.JButton jButton3;
    private javax.swing.JButton jButton4;
    private javax.swing.JLabel jLabel1;
    private javax.swing.JMenu jMenu1;
    private javax.swing.JMenu jMenu2;
    private javax.swing.JMenuBar jMenuBar1;
}


```
## practica5application
```java
package com.example.nico.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Practica5Application {

	public static void main(String[] args) {
		SpringApplication.run(Practica5Application.class, args);
	}

}

```