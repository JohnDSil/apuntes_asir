# 6. Administración de Oracle Database y Programación PL/SQL

En entornos empresariales, **Oracle Database** es uno de los motores relacionales más robustos. En ASGBD se aborda tanto la administración de instancias y usuarios multitenant (CDB/PDB) como la programación de lógica de negocio en servidor mediante **PL/SQL**.

---

## 1. Conexión y Gestión de Usuarios en Oracle Express (XE)

Desde Oracle 12c en adelante, el motor utiliza una arquitectura **Multitenant**:
- **CDB (*Container Database*):** Base de datos contenedora raíz (`CDB$ROOT`).
- **PDB (*Pluggable Database*):** Bases de datos conectables independientes donde residen las aplicaciones y esquemas de usuario (por defecto en Oracle XE se llama **`xepdb1`**).

```mermaid
flowchart TD
    CDB["CDB$ROOT (Base de Datos Contenedora)"] --> PDB1["PDB: xepdb1 (Servicio Pluggable)"]
    PDB1 --> UserA["Esquema: alumno"]
    PDB1 --> UserB["Esquema: empresa_db"]
```

### Protocolo de Creación y Configuración de un Usuario
Para que un usuario pueda conectarse y crear tablas en Oracle, deben ejecutarse los siguientes pasos desde el usuario administrativo **`system`**:

```sql
-- 1. Conexión al servicio pluggable xepdb1 con el usuario system
-- sqlplus system/contraseña@//localhost:1521/xepdb1

-- 2. Creación del usuario con su contraseña
CREATE USER nombre_usuario IDENTIFIED BY password_usuario;

-- 3. Concesión de roles básicos del sistema
GRANT connect, resource TO nombre_usuario;

-- 4. Asignación de cuota de almacenamiento ilimitada en el tablespace USERS
ALTER USER nombre_usuario QUOTA UNLIMITED ON USERS;
```

> [!IMPORTANT] **El rol de la cuota en tablespaces (Quota on USERS)**
> Aunque al usuario se le conceda el rol `RESOURCE`, **por defecto no tiene cuota asignada en disco**. Si no se ejecuta `ALTER USER ... QUOTA UNLIMITED ON USERS`, cualquier intento de `CREATE TABLE` o `INSERT` fallará con el error `ORA-01950: no privileges on tablespace 'USERS'`.

---

## 2. Programación en Servidor: Oracle PL/SQL

**PL/SQL (*Procedural Language / SQL*)** es la extensión procedural de Oracle que permite combinar la potencia declarativa de SQL con estructuras de control de flujo (bucles, condicionales, variables y control de excepciones).

### Estructura de un Bloque PL/SQL:
```sql
DECLARE
    -- Declaración de variables, constantes y tipos
    v_nombre VARCHAR2(50);
BEGIN
    -- Cuerpo ejecutable con sentencias SQL y lógica procedural
    SELECT ename INTO v_nombre FROM emp WHERE empno = 7839;
    DBMS_OUTPUT.PUT_LINE('Empleado: ' || v_nombre);
EXCEPTION
    -- Captura y tratamiento de errores en tiempo de ejecución
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No se encontró el empleado solicitado.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error imprevisto: ' || SQLERRM);
END;
/
```

---

## 3. Procedimientos vs. Funciones Almacenadas

| Característica | Procedimiento (`PROCEDURE`) | Función (`FUNCTION`) |
|---|---|---|
| **Propósito** | Ejecutar una acción o conjunto de operaciones. | Calcular y retornar un valor específico. |
| **Sentencia `RETURN`** | Opcional (solo para terminar la ejecución antes). | **Obligatoria** en la cabecera y en el cuerpo. |
| **Llamada en SQL** | Desde bloques PL/SQL o con `EXECUTE`. | **Directamente en sentencias `SELECT` o `WHERE`**. |
| **Parámetros** | Admite `IN`, `OUT` e `IN OUT`. | Típicamente solo `IN`. |

### Ejemplo: Procedimiento Almacenado
```sql
CREATE OR REPLACE PROCEDURE calcular_antiguedad (
    p_empno IN emp.empno%TYPE
)
IS
    v_ename    emp.ename%TYPE;
    v_hiredate emp.hiredate%TYPE;
    v_anios    NUMBER;
BEGIN
    SELECT ename, hiredate
    INTO v_ename, v_hiredate
    FROM emp
    WHERE empno = p_empno;

    v_anios := TRUNC(MONTHS_BETWEEN(SYSDATE, v_hiredate) / 12);
    DBMS_OUTPUT.PUT_LINE('Empleado: ' || v_ename || ' - Antigüedad: ' || v_anios || ' años');
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('El código de empleado no existe.');
END calcular_antiguedad;
/
```

### Ejemplo: Función Almacenada
```sql
CREATE OR REPLACE FUNCTION obtener_salario_anual (
    p_salario emp.sal%TYPE,
    p_comision emp.comm%TYPE
)
RETURN NUMBER
IS
BEGIN
    RETURN (p_salario * 12) + NVL(p_comision, 0);
END obtener_salario_anual;
/

-- Uso directo en consultas SELECT cotidianas:
SELECT ename, sal, obtener_salario_anual(sal, comm) AS "Salario Anual Bruto"
FROM emp;
```

---

## 4. Disparadores de Base de Datos (Triggers)

Un **trigger** es un bloque de código PL/SQL que se ejecuta automáticamente cuando ocurre un evento de modificación (`INSERT`, `UPDATE` o `DELETE`) sobre una tabla.

```sql
-- Trigger para auditar cambios salariales a nivel de fila
CREATE OR REPLACE TRIGGER trg_auditoria_salario
BEFORE UPDATE OF sal ON emp
FOR EACH ROW
BEGIN
    -- :OLD contiene el valor antes de la actualización
    -- :NEW contiene el valor propuesto tras la actualización
    IF :NEW.sal < :OLD.sal THEN
        RAISE_APPLICATION_ERROR(-20001, 'No se permite reducir el salario de un empleado.');
    END IF;
END;
/
```
