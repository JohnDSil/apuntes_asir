# 3. Funciones de Grupo y Agrupamiento (GROUP BY / HAVING)

Las **funciones de grupo o de agregación** operan sobre conjuntos de filas para calcular un único valor representativo por conjunto (totales, medias, máximos o recuentos).

---

## 1. Catálogo de Funciones de Agregación

```mermaid
graph LR
    F["Funciones de Grupo"]
    F --> Num["Solo Numéricas: AVG(), SUM()"]
    F --> Gen["Cualquier Tipo (Num, Char, Date): MIN(), MAX()"]
    F --> Count["Recuento: COUNT(*), COUNT(expr)"]
    F --> Stat["Estadísticas: STDDEV(), VARIANCE()"]
```

| Función | Descripción | Tipos de datos admitidos |
|---|---|---|
| **`AVG([DISTINCT] n)`** | Calcula la media aritmética de los valores. Ignora los valores nulos. | Exclusivamente numéricos. |
| **`SUM([DISTINCT] n)`** | Obtiene la sumatoria total de los valores del grupo. | Exclusivamente numéricos. |
| **`MIN([DISTINCT] expr)`** | Devuelve el valor mínimo (alfabético, cronológico o numérico). | Cualquier tipo (`NUMBER`, `VARCHAR2`, `DATE`). |
| **`MAX([DISTINCT] expr)`** | Devuelve el valor máximo (alfabético, cronológico o numérico). | Cualquier tipo (`NUMBER`, `VARCHAR2`, `DATE`). |
| **`COUNT(*)`** | Devuelve el número total de registros de la tabla o grupo (incluyendo nulos y duplicados). | Todas las filas. |
| **`COUNT(expr)`** | Devuelve el número de registros donde la expresión **no es nula** (`IS NOT NULL`). | Cualquier tipo. |
| **`STDDEV(x)`** | Calcula la desviación típica de la muestra. | Exclusivamente numéricos. |
| **`VARIANCE(x)`** | Calcula la varianza estadística de la muestra. | Exclusivamente numéricos. |

```sql
-- Consultas resumen a nivel de toda la tabla
SELECT AVG(sal), MAX(sal), MIN(sal), SUM(sal)
FROM emp
WHERE job LIKE 'SALES%';

-- MIN y MAX aplicados sobre tipos DATE (fechas de contratación)
SELECT MIN(hiredate) AS "Primer Contratado", MAX(hiredate) AS "Último Contratado"
FROM emp;
```

---

## 2. Tratamiento de Valores Nulos (`NULL`)

> [!WARNING] **Comportamiento por defecto con valores NULL**
> **Todas las funciones de grupo ignoran automáticamente los valores nulos** de las columnas evaluadas (con la única excepción de `COUNT(*)`).

### Ejemplo práctico:
En una empresa con 14 empleados, solo 4 cobran comisión (`comm`):
- `COUNT(*)` devuelve **14** (total de filas).
- `COUNT(comm)` devuelve **4** (solo filas con comisión informada).
- `AVG(comm)` sumará las 4 comisiones y dividirá entre **4** (resultado: $550$).

### ¿Cómo forzar el cálculo incluyendo los nulos como 0?
Se emplean funciones de sustitución de nulos para tratar la ausencia de dato como valor neutro ($0$):

=== "Oracle Database (NVL)"
    ```sql
    -- NVL(comm, 0) sustituye NULL por 0 antes de calcular la media, dividiendo entre 14
    SELECT AVG(NVL(comm, 0)) AS "Media Real Plantilla"
    FROM emp;
    -- Resultado: 157.14
    ```

=== "MySQL / MariaDB (IFNULL / COALESCE)"
    ```sql
    SELECT AVG(IFNULL(comm, 0)) AS "Media Real Plantilla"
    FROM emp;

    -- O estándar ANSI:
    SELECT AVG(COALESCE(comm, 0)) AS "Media Real Plantilla"
    FROM emp;
    ```

---

## 3. Creación de Grupos: Cláusula `GROUP BY`

Mientras que las consultas resumen calculan un único total general, la cláusula **`GROUP BY`** segmenta los registros en grupos homogéneos para calcular subtotales independientes por categoría.

```mermaid
flowchart LR
    subgraph Registros ["14 Empleados (Tabla EMP)"]
        R1["Dept 10 (3 emp)"]
        R2["Dept 20 (5 emp)"]
        R3["Dept 30 (6 emp)"]
    end
    Registros -->|"GROUP BY deptno"| Agrupado
    subgraph Agrupado ["Subtotales por Departamento"]
        G1["Dept 10 -> AVG(sal) = 2916.66"]
        G2["Dept 20 -> AVG(sal) = 2175.00"]
        G3["Dept 30 -> AVG(sal) = 1566.67"]
    end
```

### 🚨 La Regla de Oro de `GROUP BY`
> **Cualquier columna o expresión que figure en la lista de selección (`SELECT`) y que NO sea una función agregada, TIENE que estar declarada obligatoriamente en la cláusula `GROUP BY`.**

```sql
-- ❌ CONSULTA INVÁLIDA: Provoca error ORA-00937 (not a single-group group function)
SELECT deptno, COUNT(ename)
FROM emp;

-- ✅ CONSULTA CORRECTA:
SELECT deptno, COUNT(ename), AVG(sal)
FROM emp
GROUP BY deptno;
```

*(Nota: La columna utilizada en el `GROUP BY` no tiene por qué aparecer obligatoriamente en el `SELECT`, aunque es una buena práctica para identificar cada fila resultante).*

### Agrupación por Múltiples Columnas
Es posible anidar niveles de agrupación separando las columnas por comas:

```sql
-- Calcula la masa salarial para cada oficio dentro de cada departamento
SELECT deptno, job, SUM(sal)
FROM emp
GROUP BY deptno, job
ORDER BY deptno, job;
```

---

## 4. Filtrado de Grupos: Cláusula `HAVING` vs. `WHERE`

Existe una distinción esencial entre filtrar filas individuales y filtrar grupos calculados:

```mermaid
flowchart TD
    Tabla["Tabla Original"] -->|"1. WHERE (Filtra filas individuales ANTES de agrupar)"| FilasFiltradas["Filas Seleccionadas"]
    FilasFiltradas -->|"2. GROUP BY (Forma los grupos)"| Grupos["Grupos Creados"]
    Grupos -->|"3. Funciones Agregadas (AVG, SUM, MAX...)"| GruposCalculados["Grupos con Métricas"]
    GruposCalculados -->|"4. HAVING (Filtra grupos DESPUÉS de la agregación)"| ResultadoFinal["Resultado en Pantalla"]
```

| Cláusula | Momento de evaluación | ¿Admite funciones de grupo? | Propósito |
|---|---|---|---|
| **`WHERE`** | **Antes** de agrupar las filas. | ❌ **PROHIBIDO** (`ORA-00934`) | Excluir registros que no deben intervenir en el cálculo. |
| **`HAVING`** | **Después** de formar los grupos y calcular agregados. | ✅ **SÍ** | Excluir grupos enteros según el resultado de su función agregada. |

> [!TIP] **¿Cuándo poner la condición en WHERE o en HAVING?**
> - Si la condición recae sobre un **atributo de un registro individual**, se debe colocar obligatoriamente en el **`WHERE`**.
> - Si la condición recae sobre un **atributo del grupo de registros o una función agregada**, se debe colocar en el **`HAVING`**.

### Orden Oficial de Evaluación del Motor SQL
En tiempo de ejecución, el motor relacional procesa las cláusulas en el siguiente orden estricto:

```mermaid
flowchart LR
    P1["1. WHERE\n(Selecciona filas individuales)"] --> P2["2. GROUP BY\n(Agrupa filas en tabla temporal)"]
    P2 --> P3["3. HAVING\n(Selecciona grupos de la temporal)"]
    P3 --> P4["4. ORDER BY\n(Ordena los grupos resultantes)"]
```

---

### Consultas Avanzadas con Subconsultas en `HAVING`

1. **Departamentos cuyo salario medio supera la media de toda la empresa:**
```sql
SELECT deptno, AVG(sal)
FROM emp
GROUP BY deptno
HAVING AVG(sal) >= (SELECT AVG(sal) FROM emp);
```

2. **Departamentos con mayor número de empleados (agregación sobre subconsulta):**
```sql
SELECT d.deptno, d.dname, COUNT(e.empno) AS total_empleados
FROM emp e, dept d
WHERE e.deptno = d.deptno
GROUP BY d.deptno, d.dname
HAVING COUNT(e.empno) = (
    SELECT MAX(COUNT(*))
    FROM emp
    GROUP BY deptno
);
```

3. **Departamentos que tienen más de 4 analistas:**
```sql
SELECT d.dname, COUNT(e.empno) AS total_analistas
FROM emp e, dept d
WHERE e.deptno = d.deptno
  AND e.job = 'ANALYST'
GROUP BY d.dname
HAVING COUNT(e.empno) > 4;
```

---

### Formateo Numérico Avanzado en Agrupaciones
En Oracle Database, se utiliza `TO_CHAR` con máscaras numéricas para presentar resúmenes ejecutivos con formato monetario y separadores de millares:

```sql
SELECT deptno,
       TO_CHAR(SUM(sal), '99G999D99') AS "Suma Salarial",
       TO_CHAR(MAX(sal), '99G999D99') AS "Salario Máximo",
       TO_CHAR(MIN(sal), '99G999D99') AS "Salario Mínimo"
FROM emp
GROUP BY deptno;
```

---

### Consulta Completa de Síntesis:
```sql
SELECT job, SUM(sal) AS "Gasto Salarial"
FROM emp
WHERE job NOT LIKE 'SALES%'       -- 1. Excluye comerciales antes de agrupar
GROUP BY job                      -- 2. Agrupa por oficio
HAVING SUM(sal) > 5000            -- 3. Solo oficios con coste total > 5000
ORDER BY SUM(sal) DESC;           -- 4. Ordena el resultado final
```

---

## 5. Anidamiento de Funciones de Grupo

En motores como **Oracle Database**, se permite anidar dos funciones de grupo para responder preguntas complejas sin subconsultas intermedias:

```sql
-- Calcula la media de salarios por departamento, y luego extrae la media más alta:
SELECT MAX(AVG(sal)) AS "Media Departamental Más Alta"
FROM emp
GROUP BY deptno;
```
