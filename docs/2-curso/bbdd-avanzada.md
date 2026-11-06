# Administración de Bases de Datos y PL/SQL

Programación de lógica de negocio en servidor con Oracle PL/SQL: procedimientos, funciones, cursores y disparadores (triggers).

---

## ⚡ Procedimientos y Funciones en Oracle PL/SQL

### Estructura de un Procedimiento
```sql
CREATE OR REPLACE PROCEDURE nombre_edad (
    p_dni IN personas.dni%TYPE
)
IS
    v_nombre personas.nombre%TYPE;
    v_fecha_nac personas.fecha_nac%TYPE;
    v_edad NUMBER;
BEGIN
    SELECT nombre, fecha_nac
    INTO v_nombre, v_fecha_nac
    FROM personas
    WHERE dni = p_dni;

    v_edad := TRUNC(MONTHS_BETWEEN(SYSDATE, v_fecha_nac) / 12);
    DBMS_OUTPUT.PUT_LINE('Nombre: ' || v_nombre || ' - Edad: ' || v_edad || ' años');
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No se encontró el DNI ' || p_dni);
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END nombre_edad;
/
```

### Diferencias clave: Procedimiento vs Función
| Característica | Procedimiento (`PROCEDURE`) | Función (`FUNCTION`) |
|---|---|---|
| **Propósito** | Ejecutar una acción | Calcular y retornar un valor |
| **Sentencia RETURN** | No requerida | **Obligatoria** con tipo de dato |
| **Llamada en SQL** | Desde bloques PL/SQL | Directamente en sentencias `SELECT` |
