# Administración de Sistemas Gestores de Bases de Datos (ASGBD)

Bienvenido a la sección de **Administración de Sistemas Gestores de Bases de Datos (ASGBD)** del 2º curso de ASIR.

Este módulo abarca desde la infraestructura y despliegue de motores de bases de datos mediante contenedores (**Docker**), hasta el diseño de consultas complejas (**JOINs, Subconsultas, Agrupamientos**), manipulación de datos con control de transacciones (**ACID**), y la administración avanzada y programación de lógica de servidor en **Oracle Database (PL/SQL)** y **MySQL/MariaDB**.

---

## 📚 Bloques Temáticos

| Bloque | Contenido principal | Fuentes y Enfoque |
|---|---|---|
| **[1. Docker y Contenedores para SGBD](01-docker-sgbd.md)** | Instalación en Ubuntu, ciclo de vida de contenedores, volúmenes de persistencia, redes, Compose y despliegue de **MariaDB** y **Oracle XE**. | Curso oficial de Docker (*pekechis*) y Guías de instalación |
| **[2. DDL, Diseño Físico y Restricciones](02-diseno-fisico-ddl-oracle.md)** | Esquemas, tipos, restricciones con nomenclatura oficial Oracle (`PK`, `FK`, `NN`, `UK`, `CK`), acciones referenciales (`RESTRICT`, `CASCADE`), motores de almacenamiento (`InnoDB`, `MyISAM`), `ALTER`, `DROP`, `TRUNCATE` y sinónimos. | Raúl Ruiz Padilla & IES María Moliner |
| **[3. Consultas Multitabla y Combinaciones (JOINs)](03-consultas-avanzadas-joins.md)** | Producto cartesiano (regla $N-1$), Equijoins, Outer joins (trampa `COUNT(*)` vs `COUNT(col)`), Self joins, anidaciones y **operadores de conjuntos (`UNION`, `INTERSECT`, `MINUS`)**. | Oracle University & Raúl Ruiz Padilla |
| **[4. Funciones de Grupo y Agrupamiento](04-agrupamiento-funciones-grupo.md)** | `AVG`, `SUM`, `COUNT(*)`, `COUNT(expr)`, `MIN`, `MAX`, tratamiento de nulos (`NVL`), orden de evaluación formal (`WHERE` $\rightarrow$ `GROUP BY` $\rightarrow$ `HAVING` $\rightarrow$ `ORDER BY`) y subconsultas en `HAVING`. | Oracle University & Raúl Ruiz Padilla |
| **[5. Subconsultas en SQL](05-subconsultas.md)** | Principios de arquitectura, mono-registro, multi-registro (`IN`), multi-columna (tuplas), cuantificadas (`ANY`/`ALL`), correlacionadas, diagnóstico de errores (`ORA-01427`) y ejercicios prácticos de catálogo. | Oracle Official & Raúl Ruiz Padilla |
| **[6. Manipulación de Datos (DML) y Transacciones ACID](06-dml-transacciones.md)** | `INSERT INTO` (datos anexados `INSERT ... SELECT`), `UPDATE`, `DELETE`, régimen de transacciones en Oracle (`AUTOCOMMIT OFF`, órdenes de `COMMIT` implícito) y caso práctico integral universitario. | Raúl Ruiz Padilla & Antonio Fdez. Japón |
| **[7. Administración de Oracle y Lógica PL/SQL](07-administracion-oracle-plsql.md)** | PDB (`xepdb1`), variables `%TYPE`/`%ROWTYPE`, control de flujo, procedimientos vs funciones (`IN`, `OUT`, `IN OUT`), ciclo de cursores, excepciones y script administrativo de limpieza total (*Drop All Objects*). | Raúl Ruiz Padilla & Guías Técnicas |

---

## 🎯 Objetivos y Competencias del Módulo

1. **Despliegue y Aislamiento:** Levantar y administrar instancias aisladas de MariaDB, MySQL y Oracle Database mediante contenedores Docker con persistencia fiable y redes personalizadas.
2. **Consultas Complejas:** Diseñar consultas analíticas eficientes combinando múltiples fuentes relacionales sin incurrir en productos cartesianos no deseados.
3. **Agregación de Información:** Elaborar informes y resúmenes estadísticos aplicando funciones de grupo sobre conjuntos de datos segmentados.
4. **Integridad Transaccional:** Garantizar la consistencia y atomicidad de las operaciones de negocio aplicando transacciones ACID en motores relacionales.
5. **Administración y Programación en Servidor:** Gestionar la seguridad, usuarios y tablespaces en Oracle Database y desarrollar lógica de negocio mediante PL/SQL.
