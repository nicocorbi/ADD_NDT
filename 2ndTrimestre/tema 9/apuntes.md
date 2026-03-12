# 🟦 TEMA 9 — MongoDB, NoSQL y MongoDB Atlas

---

# 📌 1. ¿Qué es NoSQL?

Las bases de datos **NoSQL** son sistemas que NO utilizan el modelo relacional tradicional.
No usan tablas con filas y columnas, sino estructuras flexibles como JSON.

### Características principales:
- Modelo **no relacional**
- **Sin esquema fijo** (flexible)
- Escalabilidad **horizontal**
- Orientadas a **grandes volúmenes de datos**
- Normalmente **open source**
- Funcionan muy bien en **clústeres**

---

# 📌 1.1 ¿Qué es un clúster?

Un **clúster** es un conjunto de servidores que trabajan juntos como si fueran uno solo.

### Características:
- Alta disponibilidad
- Escalabilidad horizontal
- Seguridad integrada
- Resiliencia ante fallos
- Balanceadores de carga
- Rendimiento y baja latencia

---

# 📌 1.2 Escalabilidad

### 🔹 Escalabilidad vertical
Aumentar recursos de un servidor:
- Más CPU
- Más RAM
- Más almacenamiento

**Ventajas:** fácil  
**Desventajas:** tiene límite y es caro

### 🔹 Escalabilidad horizontal
Añadir más servidores al sistema.

**Ventajas:** casi ilimitado, más barato  
**Desventajas:** más complejo de configurar

---

# 📌 2. ¿Por qué surgieron las bases de datos NoSQL?

Para solucionar limitaciones de SQL:

- Manejo de **BIG DATA**
- Necesidad de **esquemas flexibles**
- Escalabilidad horizontal
- Datos no estructurados
- Alta disponibilidad
- Aplicaciones en tiempo real

---

# 📌 3. Tipos de bases de datos NoSQL

### 🔹 1. Clave-valor
Ej: Redis  
Ideal para cachés, sesiones, configuraciones.

### 🔹 2. Documentos (MongoDB)
Almacenan datos en JSON/BSON.  
Estructura flexible.

### 🔹 3. Columnas
Ej: Cassandra  
Optimizado para análisis de grandes datos.

### 🔹 4. Grafos
Ej: Neo4j  
Para relaciones complejas (redes sociales, rutas).

---

# 📌 4. Teorema de CAP

En un sistema distribuido NO se pueden garantizar simultáneamente:

- **C**onsistencia  
- **A**vailability (Disponibilidad)  
- **P**artition Tolerance (Tolerancia a particiones)

Solo se pueden cumplir **2 a la vez**.

### Clasificación:
- **CA** → SQL (consistencia + disponibilidad)
- **CP** → MongoDB (consistencia + tolerancia a particiones)
- **AP** → Bases de datos de grafos (disponibilidad + tolerancia)

---

# 📌 5. MongoDB

MongoDB es una base de datos NoSQL orientada a documentos.

### Características:
- Datos en JSON/BSON
- Flexible
- Escalable horizontalmente
- Usa **replica sets**
- Muy rápida y eficiente
- Ideal para aplicaciones modernas

---

# 📌 6. MongoDB Atlas

MongoDB Atlas es la versión en la nube de MongoDB.

### Ventajas:
- No requiere instalación
- Alta disponibilidad
- Backups automáticos
- Escalabilidad automática
- Seguridad integrada
- Multi-región
- String de conexión para aplicaciones

---

# 📌 7. Elementos de MongoDB

### 🔹 Base de datos
Conjunto de colecciones.

### 🔹 Colección
Equivalente a una tabla, pero sin esquema fijo.

### 🔹 Documento
Equivalente a una fila, pero en formato JSON.

---

# 📌 8. Spring Boot + MongoDB Atlas

Para conectar una app Java con MongoDB Atlas:

### En `application.properties`:


spring.data.mongodb.uri=mongodb+srv://usuario:password@cluster.mongodb.net/NombreBD?retryWrites=true&w=majority

---

# 📌 9. Anotaciones importantes

### @Document
Indica que una clase es un documento de MongoDB.

### @Id
Clave primaria del documento.

### @Field
Permite cambiar el nombre del campo en MongoDB.

---

# 📌 10. Repositorios en Spring Boot

Se usa:

```java
public interface TransactionRepository extends MongoRepository<Transaction, String> {}
Métodos personalizados (keywords):
findByIncomeTrue()

findByIncomeFalse()

findByQuantityGreaterThan()

findByQuantityBetween()

findByDescriptionContaining()

findByIncomeFalseAndDateBetween()
```
# 📌 11. CommandLineRunner
Permite ejecutar código al iniciar la aplicación.

Ejemplo:
```java
java
@Override
public void run(String... args) throws Exception {
    // Código que se ejecuta al arrancar
}
```
# 📌 12. Práctica: Aplicación de gastos e ingresos
Modelo Transaction:
id: String

description: String

quantity: double

income: boolean

date: Date

Funcionalidades:
Insertar transacciones

Ver ingresos

Ver gastos

Buscar por palabra

Buscar por cantidad

Filtrar entre fechas

Mostrar resultados en consola

Ver datos en MongoDB Atlas
<img width="1920" height="1200" alt="Captura de pantalla 2026-03-05 102303" src="https://github.com/user-attachments/assets/a4577f9d-b2b5-4a05-a3ea-63ea0242f267" />
<img width="1920" height="1200" alt="Captura de pantalla 2026-03-05 100946" src="https://github.com/user-attachments/assets/85cbe4da-951a-4dd8-a99c-b103a22e25dc" />
<img width="1920" height="1200" alt="Captura de pantalla 2026-03-05 100204" src="https://github.com/user-attachments/assets/931ebccb-7813-4633-9f69-d1da52d4e10d" />
<img width="1920" height="1200" alt="Captura de pantalla 2026-03-05 095600" src="https://github.com/user-attachments/assets/58d76ba3-c7d8-4869-82c5-c07a562c8d43" />
<img width="1920" height="1200" alt="Captura de pantalla 2026-03-05 095253" src="https://github.com/user-attachments/assets/69597a64-648b-4228-a649-ac7c481706a7" />
<img width="1920" height="1200" alt="Captura de pantalla 2026-03-05 095104" src="https://github.com/user-attachments/assets/a425f7ad-aa61-4f7e-b012-75ca300d1ba7" />
<img width="1920" height="1200" alt="Captura de pantalla 2026-03-12 100307" src="https://github.com/user-attachments/assets/3028e99f-37a5-4c0c-9476-e70aa2ce10f8" />
<img width="1920" height="1200" alt="Captura de pantalla 2026-03-12 100300" src="https://github.com/user-attachments/assets/2a36bc05-6e53-476e-9d62-878bce195c65" />
<img width="1920" height="1200" alt="Captura de pantalla 2026-03-12 095616" src="https://github.com/user-attachments/assets/939622c9-4ddb-405d-bf17-91da09de4976" />
<img width="1920" height="1200" alt="Captura de pantalla 2026-03-12 095420" src="https://github.com/user-attachments/assets/db6958fc-8240-4c86-949a-c84583b64227" />
<img width="1920" height="1200" alt="Captura de pantalla 2026-03-05 110032" src="https://github.com/user-attachments/assets/f77ec7f0-9254-47b8-b410-170d76af7de0" />

