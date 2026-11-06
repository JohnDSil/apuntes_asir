# Gestión de Bases de Datos (GBD)

Diseño conceptual, lógico y físico de bases de datos relacionales, lenguaje SQL y administración básica.

---

## 💾 Consultas SQL (DML)

```sql
-- Creación de tabla con clave foránea
CREATE TABLE departamentos (
    id_dept INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50) NOT NULL
);

CREATE TABLE empleados (
    id_emp INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50) NOT NULL,
    salario DECIMAL(10,2),
    id_dept INT,
    FOREIGN KEY (id_dept) REFERENCES departamentos(id_dept)
);

-- Consultas con JOIN
SELECT e.nombre, e.salario, d.nombre AS departamento
FROM empleados e
INNER JOIN departamentos d ON e.id_dept = d.id_dept
WHERE e.salario > 1800
ORDER BY e.salario DESC;
```
