# Administración de Sistemas Gestores de Bases de Datos (ASGBD)

Bienvenido a la sección de **Administración de Sistemas Gestores de Bases de Datos (ASGBD)** del 2º curso de ASIR.

Este módulo abarca desde la infraestructura y despliegue de motores de bases de datos mediante contenedores (**Docker**), hasta el diseño de consultas complejas (**JOINs, Subconsultas, Agrupamientos**), manipulación de datos con control de transacciones (**ACID**), y la administración avanzada y programación de lógica de servidor en **Oracle Database (PL/SQL)** y **MySQL/MariaDB**.

---

## 📚 Bloques Temáticos

| Bloque | Contenido principal | Fuentes y Enfoque |
|---|---|---|
| **[1. Docker y Contenedores para SGBD](01-docker-sgbd.md)** | Arquitectura de contenedores, ciclo de vida, persistencia (volúmenes vs bind mounts), redes virtuales, Dockerfile, Docker Compose multicapa y seguridad. | Basado en el curso oficial de Docker de la Consejería de Educación (*pekechis*). |
| **[2. Consultas Multitabla y Combinaciones (JOINs)](02-consultas-avanzadas-joins.md)** | Producto cartesiano, Equijoins, Non-equijoins (`BETWEEN`), Outer joins (sintaxis Oracle `(+)` y ANSI `LEFT/RIGHT JOIN`), Self joins y anidaciones. | Oracle University & Antonio Fdez. Japón |
| **[3. Funciones de Grupo y Agrupamiento](03-agrupamiento-funciones-grupo.md)** | `AVG`, `SUM`, `COUNT(*)`, `COUNT(expr)`, `MIN`, `MAX`, tratamiento de valores `NULL` (`NVL`), cláusula `GROUP BY`, filtrado con `HAVING` y anidamientos. | Oracle University & Antonio Fdez. Japón |
| **[4. Subconsultas en SQL](04-subconsultas.md)** | Subconsultas escalares, de lista (`IN`/`NOT IN`), modificadores `ANY`/`ALL`, comprobación de existencia con `EXISTS`/`NOT EXISTS` y subconsultas en expresiones. | Antonio Fernández Japón |
| **[5. Manipulación de Datos (DML) y Transacciones ACID](05-dml-transacciones.md)** | `INSERT INTO` (simple, múltiple y con `SELECT`), `UPDATE`, `DELETE`, transacciones atómicas, motores transaccionales (`InnoDB`), `COMMIT` y `ROLLBACK`. | Antonio Fernández Japón |
| **[6. Administración de Oracle y Lógica PL/SQL](06-administracion-oracle-plsql.md)** | Conexión a PDB (`xepdb1`), creación de usuarios, roles `CONNECT`/`RESOURCE`, cuotas de almacenamiento y programación de Procedimientos, Funciones y Triggers en PL/SQL. | Guías Oracle XE & PL/SQL avanzado |

---

## 🎯 Objetivos y Competencias del Módulo

1. **Despliegue y Aislamiento:** Levantar y administrar instancias aisladas de MariaDB, MySQL y Oracle Database mediante contenedores Docker con persistencia fiable y redes personalizadas.
2. **Consultas Complejas:** Diseñar consultas analíticas eficientes combinando múltiples fuentes relacionales sin incurrir en productos cartesianos no deseados.
3. **Agregación de Información:** Elaborar informes y resúmenes estadísticos aplicando funciones de grupo sobre conjuntos de datos segmentados.
4. **Integridad Transaccional:** Garantizar la consistencia y atomicidad de las operaciones de negocio aplicando transacciones ACID en motores relacionales.
5. **Administración y Programación en Servidor:** Gestionar la seguridad, usuarios y tablespaces en Oracle Database y desarrollar lógica de negocio mediante PL/SQL.
