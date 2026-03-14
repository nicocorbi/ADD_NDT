## EN ESTA PRACTICA HEMOS USADO NET BEANS
## Practica9Application.java
```java
package com.example.practica9;


import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;
import java.util.Scanner;

@SpringBootApplication
public class Practica9Application implements CommandLineRunner {

    @Autowired
    private TransactionRepository transactionRepository;

    public static void main(String[] args) {
        SpringApplication.run(Practica9Application.class, args);
    }

    @Override
    public void run(String... args) throws Exception {

        SimpleDateFormat sdf = new SimpleDateFormat("dd/MM/yyyy");

        

        // 2. Menú interactivo
        Scanner sc = new Scanner(System.in);
        int opcion = -1;

        System.out.println("\n=====================================");
        System.out.println("   SISTEMA DE GESTIÓN DE GASTOS");
        System.out.println("=====================================");

        while (opcion != 0) {

            System.out.println("\n===== MENÚ PRINCIPAL =====");
            System.out.println("1.  Añadir transacción");
            System.out.println("2.  Ver ingresos");
            System.out.println("3.  Ver gastos");
            System.out.println("4.  Ver transacciones de un mes");
            System.out.println("5.  Ver transacciones con valor superior a X");
            System.out.println("6.  Ver transacciones con valor entre X e Y");
            System.out.println("7.  Ver gastos entre dos fechas");
            System.out.println("8.  Ver gastos con cantidad menor a X");
            System.out.println("9.  Ver gastos con cantidad mayor a X");
            System.out.println("10. Buscar transacciones que contengan una palabra");
            System.out.println("11. Ver todas las transacciones");
            System.out.println("0.  Salir");
            System.out.print("Elige una opción: ");

            opcion = sc.nextInt();
            sc.nextLine();

            switch (opcion) {

                case 1:
                    System.out.println("\n--- AÑADIR TRANSACCIÓN ---");

                    System.out.print("Descripción: ");
                    String desc = sc.nextLine();

                    System.out.print("Cantidad: ");
                    double cantidad = sc.nextDouble();
                    sc.nextLine();

                    System.out.print("¿Es ingreso? (true/false): ");
                    boolean income = sc.nextBoolean();
                    sc.nextLine();

                    Date fecha = null;
                    while (fecha == null) {
                        System.out.print("Fecha (dd/MM/yyyy): ");
                        String fechaStr = sc.nextLine();
                        try {
                            fecha = sdf.parse(fechaStr);
                        } catch (Exception e) {
                            System.out.println("Formato incorrecto, usa dd/MM/yyyy. Ejemplo: 12/03/2026");
                        }
                    }

                    Transaction t = new Transaction(desc, cantidad, income, fecha);
                    transactionRepository.save(t);
                    System.out.println("Transacción guardada correctamente.");
                    break;

                case 2:
                    System.out.println("\n--- INGRESOS ---");
                    transactionRepository.findByIncomeTrue().forEach(System.out::println);
                    break;

                case 3:
                    System.out.println("\n--- GASTOS ---");
                    transactionRepository.findByIncomeFalse().forEach(System.out::println);
                    break;

                case 4:
                    System.out.print("Introduce el mes (1-12): ");
                    int mes = sc.nextInt();
                    System.out.print("Introduce el año (ej: 2026): ");
                    int anio = sc.nextInt();
                    sc.nextLine();

                    Calendar calInicio = Calendar.getInstance();
                    calInicio.set(anio, mes - 1, 1, 0, 0, 0);
                    calInicio.set(Calendar.MILLISECOND, 0);

                    Calendar calFin = Calendar.getInstance();
                    calFin.set(anio, mes - 1, 1, 23, 59, 59);
                    calFin.set(Calendar.DAY_OF_MONTH, calFin.getActualMaximum(Calendar.DAY_OF_MONTH));
                    calFin.set(Calendar.MILLISECOND, 999);

                    System.out.println("\n--- TRANSACCIONES DEL MES " + mes + "/" + anio + " ---");
                    transactionRepository.findByDateBetween(calInicio.getTime(), calFin.getTime())
                            .forEach(System.out::println);
                    break;

                case 5:
                    System.out.print("Valor mínimo: ");
                    double valorMin = sc.nextDouble();
                    sc.nextLine();
                    System.out.println("\n--- TRANSACCIONES CON VALOR SUPERIOR A " + valorMin + " ---");
                    transactionRepository.findByQuantityGreaterThan(valorMin).forEach(System.out::println);
                    break;

                case 6:
                    System.out.print("Valor mínimo: ");
                    double entre1 = sc.nextDouble();
                    System.out.print("Valor máximo: ");
                    double entre2 = sc.nextDouble();
                    sc.nextLine();
                    System.out.println("\n--- TRANSACCIONES ENTRE " + entre1 + " Y " + entre2 + " ---");
                    transactionRepository.findByQuantityBetween(entre1, entre2).forEach(System.out::println);
                    break;

                case 7:
                    Date fechaInicio = null;
                    Date fechaFin = null;

                    while (fechaInicio == null) {
                        System.out.print("Fecha inicio (dd/MM/yyyy): ");
                        try {
                            fechaInicio = sdf.parse(sc.nextLine());
                        } catch (Exception e) {
                            System.out.println("Formato incorrecto, usa dd/MM/yyyy.");
                        }
                    }
                    while (fechaFin == null) {
                        System.out.print("Fecha fin (dd/MM/yyyy): ");
                        try {
                            fechaFin = sdf.parse(sc.nextLine());
                        } catch (Exception e) {
                            System.out.println("Formato incorrecto, usa dd/MM/yyyy.");
                        }
                    }
                    System.out.println("\n--- GASTOS ENTRE FECHAS ---");
                    transactionRepository.findByIncomeFalseAndDateBetween(fechaInicio, fechaFin)
                            .forEach(System.out::println);
                    break;

                case 8:
                    System.out.print("Cantidad máxima: ");
                    double cantMax = sc.nextDouble();
                    sc.nextLine();
                    System.out.println("\n--- GASTOS CON CANTIDAD MENOR A " + cantMax + " ---");
                    transactionRepository.findByIncomeFalseAndQuantityLessThan(cantMax).forEach(System.out::println);
                    break;

                case 9:
                    System.out.print("Cantidad mínima: ");
                    double cantMin = sc.nextDouble();
                    sc.nextLine();
                    System.out.println("\n--- GASTOS CON CANTIDAD MAYOR A " + cantMin + " ---");
                    transactionRepository.findByIncomeFalseAndQuantityGreaterThan(cantMin).forEach(System.out::println);
                    break;

                case 10:
                    System.out.print("Palabra a buscar: ");
                    String palabra = sc.nextLine();
                    System.out.println("\n--- RESULTADOS ---");
                    transactionRepository.findByDescriptionContaining(palabra).forEach(System.out::println);
                    break;

                case 11:
                    System.out.println("\n--- TODAS LAS TRANSACCIONES ---");
                    transactionRepository.findAll().forEach(System.out::println);
                    break;

                case 0:
                    System.out.println("Saliendo del programa...");
                    break;

                default:
                    System.out.println("Opción no válida.");
            }
        }

        sc.close();
    }
}
```
### Transaction.java
```java
package com.example.practica9;

import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;
import java.util.Date;

@Document(collection = "AccesoADatosCollection")
public class Transaction {

    @Id
    private String id;
    private String description;
    private double quantity;
    private boolean income;
    private Date date;

    public Transaction() {}

    public Transaction(String description, double quantity, boolean income, Date date) {
        this.description = description;
        this.quantity = quantity;
        this.income = income;
        this.date = date;
    }

    public String getId() { return id; }
    public void setId(String id) { this.id = id; }

    public String getDescription() { return description; }
    public void setDescription(String description) { this.description = description; }

    public double getQuantity() { return quantity; }
    public void setQuantity(double quantity) { this.quantity = quantity; }

    public boolean isIncome() { return income; }
    public void setIncome(boolean income) { this.income = income; }

    public Date getDate() { return date; }
    public void setDate(Date date) { this.date = date; }

    @Override
    public String toString() {
        return "Transaction{id='" + id + "', description='" + description +
               "', quantity=" + quantity + ", income=" + income + ", date=" + date + "}";
    }
}
```
## TransactionRepository
```java
package com.example.practica9;

import org.springframework.data.mongodb.repository.MongoRepository;
import java.util.Date;
import java.util.List;

public interface TransactionRepository extends MongoRepository<Transaction, String> {

    List<Transaction> findByIncomeFalse();
    List<Transaction> findByIncomeTrue();
    List<Transaction> findByQuantityGreaterThan(double quantity);
    List<Transaction> findByQuantityBetween(double min, double max);
    List<Transaction> findByDateBetween(Date start, Date end);
    List<Transaction> findByIncomeFalseAndDateBetween(Date start, Date end);
    List<Transaction> findByIncomeFalseAndQuantityLessThan(double quantity);
    List<Transaction> findByIncomeFalseAndQuantityGreaterThan(double quantity);
    List<Transaction> findByDescriptionContaining(String keyword);
}
```

## Properties
```java
spring.application.name=practica9
spring.data.mongodb.uri=mongodb+srv://nico_db_user:RqZImcYJa4aJcrQo@accesoadatos0.jnnhebo.mongodb.net/practica9?retryWrites=true&w=majority&appName=AccesoADatos0&tlsInsecure=true
```