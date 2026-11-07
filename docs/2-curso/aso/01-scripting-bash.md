# Scripting en Bash y Automatización CLI

En la administración de sistemas operativos Linux/UNIX, el **Shell Scripting** con GNU Bash es la habilidad fundamental para automatizar tareas repetitivas, gestionar configuraciones, monitorizar servicios y crear herramientas operativas robustas y seguras.

---

## 1. Fundamentos de un Script en Bash

Un script de Bash es un archivo de texto plano que contiene una secuencia de órdenes ejecutadas secuencialmente por el intérprete `/bin/bash`.

### 1.1 Directiva Shebang (`#!`)
La primera línea de cualquier script profesional debe especificar el intérprete exacto mediante el **shebang**:

```bash
#!/bin/bash
```

> [!NOTE] ¿Por qué especificar `/bin/bash` en lugar de `/bin/sh`?
> En sistemas Debian/Ubuntu, `/bin/sh` apunta a `dash` (Debian Almquist Shell), una shell POSIX rápida y ligera pero carente de "bashismos" como `[[ ... ]]`, arrays o expansiones avanzadas. Indicar explícitamente `#!/bin/bash` asegura total compatibilidad con todas las características de Bash.

### 1.2 Métodos de Ejecución y Permisos

Para ejecutar un script existen dos formas principales:

1. **Invocación directa a través del intérprete**:
   ```bash
   bash miscript.sh
   # O con sh:
   sh miscript.sh
   ```
   No requiere que el archivo tenga permisos de ejecución, únicamente permiso de lectura (`r`).

2. **Ejecución como binario autónomo (Recomendado)**:
   ```bash
   chmod +x miscript.sh
   ./miscript.sh
   ```
   Requiere permisos de lectura y ejecución (`rx`). Si el script se ubica en un directorio dentro del `$PATH` (como `/usr/local/bin` o `~/bin`), podrá ejecutarse desde cualquier ruta indicando únicamente su nombre.

```
+----------------------------------------------------------------+
|                     PRINCIPIO KISS EN BASH                     |
|           "Keep It Simple, Stupid" (Mantenlo simple)           |
|  - Escribe código legible, estructurado y documentado.         |
|  - Valida siempre los argumentos y el usuario de ejecución.    |
|  - Comprueba los códigos de retorno ($?) tras acciones clave.  |
+----------------------------------------------------------------+
```

---

## 2. Depuración de Scripts (Debugging)

El comando interno `set` de Bash permite activar banderas de depuración para rastrear errores sintácticos y lógicos:

| Opción | Modo | Descripción |
| :--- | :--- | :--- |
| `set -n` | `noexec` | Lee los comandos pero **no los ejecuta**. Útil para detectar errores de sintaxis antes de lanzar operaciones destructivas. |
| `set -v` | `verbose` | Imprime las líneas del script por pantalla tal y como se leen en el archivo, antes de ejecutar. |
| `set -x` | `xtrace` | Muestra cada comando **tras expandir** variables, comodines y sustituciones, precedido por el valor de `$PS4` (por defecto `+ `). |
| `set -e` | `errexit` | Detiene inmediatamente la ejecución si cualquier comando devuelve un código de salida distinto de 0. |
| `set -u` | `nounset` | Trata el uso de variables no definidas como un error y aborta el script. |

### Ejemplo Práctico de Depuración con `PS4`

Podemos personalizar la variable de entorno `$PS4` para que muestre el número de línea y el nombre del script durante la traza con `-x`:

```bash
#!/bin/bash
export PS4='+ [${BASH_SOURCE}:${LINENO}]: '
set -x  # Activar traza detallada

NOMBRE="ASIR"
echo "Hola, $NOMBRE"

set +x  # Desactivar traza para el resto del código
echo "Fin de la depuración"
```

También es posible invocar la depuración directamente desde la terminal sin alterar el archivo:
```bash
bash -x miscript.sh       # Traza completa de ejecución
bash -n miscript.sh       # Verificación sintáctica sin ejecución
```

---

## 3. Salida por Pantalla y Entrecomillado

### 3.1 El comando `echo`

- `-n`: Suprime el salto de línea final.
- `-e`: Habilita la interpretación de caracteres de escape especiales (`\n` nueva línea, `\t` tabulador, `\a` alerta sonora, `\\` barra invertida):

```bash
echo -n "Introduce tu IP: "
echo -e "\nServidores activos:\n\t192.168.1.1\n\t192.168.1.10"
```

### 3.2 Reglas de Entrecomillado

- **Comillas Simples (`'...'`)**: Preservan el valor **literal** de cada carácter dentro de ellas. Anulan la expansión de variables (`$VAR`), caracteres de escape y comodines.
- **Comillas Dobles (`"..."`)**: Permiten la sustitución de variables y la ejecución de comandos, pero protegen los espacios en blanco para evitar división de palabras (*word splitting*).
- **Sin comillas**: La shell aplica expansión y división por espacios inmediatamente.

```bash
CURSO="ASO"
echo 'El módulo es $CURSO'   # Salida: El módulo es $CURSO
echo "El módulo es $CURSO"   # Salida: El módulo es ASO
```

### 3.3 Sustitución de Comandos

Permite capturar la salida estándar de un comando para guardarla en una variable:

```bash
# Sintaxis moderna y anidable (Recomendada):
FECHA_HOY=$(date +%Y-%m-%d)
TOTAL_USUARIOS=$(who | wc -l)

# Sintaxis clásica obsoleta (Backticks - Desaconsejada en código moderno):
HORA=`date +%H:%M`
```

### 3.4 Metacaracteres y Globbing

- **Metacaracteres de control**:
  - `;`: Separador secuencial de órdenes en la misma línea.
  - `&`: Ejecución en segundo plano (background).
  - `&&`: Operador lógico AND (ejecuta el siguiente comando solo si el anterior tuvo éxito, retorno 0).
  - `||`: Operador lógico OR (ejecuta el siguiente comando solo si el anterior falló, retorno distinto de 0).
  - `|`: Tubería (*pipe*), conecta la salida estándar de un proceso con la entrada del siguiente.
  - `\`: Carácter de escape para anular el significado especial del siguiente símbolo.
- **Expansiones de Ficheros (Globbing)**:
  - `*`: Coincide con cero o más caracteres (`ls *.txt`).
  - `?`: Coincide con exactamente un carácter (`cat test?.log`).
  - `[...]`: Rango de caracteres (`[0-9]`, `[a-z]`, `[A-Z]`).
  - `{a,b,c}`: Expansión de llaves (`mkdir -p /srv/{backup,data,logs}`).

---

## 4. Variables, Ámbito y Parámetros

### 4.1 Asignación y Lectura de Datos

En Bash la asignación de variables **nunca debe contener espacios** alrededor del signo igual (`=`):

```bash
SERVIDOR="srv-debian"     # Correcto
SERVIDOR = "srv-debian"   # ERROR: La shell intenta ejecutar el comando 'SERVIDOR'
```

Para solicitar datos al usuario por teclado se utiliza `read`:
```bash
read -p "Introduce el nombre de usuario a crear: " NUEVO_USUARIO
read -s -p "Introduce la contraseña: " PASSWORD   # -s oculta la entrada (password)
echo ""
```

> [!TIP] Variable `$IFS` (Internal Field Separator)
> Determina cómo Bash reconoce los delimitadores de campos al dividir cadenas y procesar lecturas. Por defecto contiene espacio, tabulador y salto de línea (`IFS=$' \t\n'`). Modificar `$IFS=":"` permite parsear fácilmente archivos como `/etc/passwd`.

### 4.2 Variables Locales vs Variables de Entorno (`export`)

- **Variable Local**: Existe únicamente dentro del proceso de la shell o script actual. No se transfiere a los procesos hijos iniciados desde él.
- **Variable de Entorno (`export`)**: Se propaga al entorno de todos los procesos hijos creados a partir del script.

```bash
MI_VAR="local"
export MI_GLOBAL="visible_en_hijos"

env          # Muestra todas las variables de entorno exportadas
set          # Muestra todas las variables (locales y de entorno) y funciones
unset MI_VAR # Elimina la variable de la memoria
```

### 4.3 Variables Especiales y Argumentos Posicionales

Bash proporciona un conjunto de variables automáticas reservadas de vital importancia para la automatización:

```mermaid
classDiagram
    class VariablesEspeciales {
        +$0 : Nombre del script invocado
        +$1..$9 : Argumentos posicionales 1 al 9
        +${10} : Argumentos a partir del décimo (entre llaves)
        +$# : Número total de argumentos pasados
        +$* : Todos los argumentos en una sola cadena
        +$@ : Todos los argumentos como elementos separados
        +$? : Código de estado del comando previo (0=OK)
        +$$ : PID del proceso del propio script
        +$! : PID del último proceso enviado a segundo plano
        +$_ : Último argumento del comando anterior
    }
```

#### Diferencia entre `"$*"` y `"$@"` entre comillas

```bash
#!/bin/bash
echo "Uso de \$*:"
for arg in "$*"; do
    echo " -> $arg"  # Se ejecuta 1 sola vez con todos los argumentos concatenados
done

echo "Uso de \$@:"
for arg in "$@"; do
    echo " -> $arg"  # Itera individualmente por cada argumento recibido
done
```

---

## 5. Operaciones Aritméticas y Matemáticas

Bash trabaja nativamente con aritmética de números enteros. Existen tres formas principales de evaluarla:

### 5.1 Sintaxis Moderna `$(( ... ))` (Recomendada)
```bash
NUM1=15
NUM2=4

SUMA=$(( NUM1 + NUM2 ))
RESTO=$(( NUM1 % NUM2 ))
POTENCIA=$(( NUM1 ** 2 ))
echo "Suma: $SUMA, Resto: $RESTO, Potencia: $POTENCIA"
```

### 5.2 Comando Interno `let`
```bash
let CONTADOR++
let TOTAL="NUM1 * 2 + 5"
```

### 5.3 Operaciones con Decimales mediante `bc -l`
Para cálculos de coma flotante o funciones matemáticas avanzadas se canaliza la expresión a la calculadora de precisión arbitraria `bc`:

```bash
# Calcular el porcentaje de uso con 2 decimales:
TOTAL_MEM=16384
USO_MEM=5420

PORCENTAJE=$(echo "scale=2; ($USO_MEM * 100) / $TOTAL_MEM" | bc -l)
echo "Uso de memoria: $PORCENTAJE %"
```

### 5.4 Utilidad `basename` y `dirname`
```bash
RUTA="/var/log/apache2/access.log"

ARCHIVO=$(basename "$RUTA")          # access.log
SIN_EXT=$(basename "$RUTA" .log)     # access
DIRECTORIO=$(dirname "$RUTA")        # /var/log/apache2
```

---

## 6. Estructuras Condicionales (`if` y `case`)

### 6.1 Sintaxis de la Sentencia `if`

```bash
if [ condicion ]; then
    # Bloque si se cumple
elif [ otra_condicion ]; then
    # Bloque alternativo
else
    # Bloque por defecto
fi
```

### 6.2 Operadores de Comparación

#### Comparación de Cadenas de Texto
| Operador | Significado |
| :--- | :--- |
| `str1 = str2` o `==` | Las cadenas son idénticas |
| `str1 != str2` | Las cadenas son diferentes |
| `-z "$str"` | Longitud cero (la cadena está vacía) |
| `-n "$str"` | Longitud mayor que cero (la cadena no está vacía) |

#### Comparación Numérica de Enteros
| Operador | Nemotécnico | Significado |
| :--- | :--- | :--- |
| `-eq` | Equal | Igual a (`==`) |
| `-ne` | Not Equal | Distinto de (`!=`) |
| `-lt` | Less Than | Menor que (`<`) |
| `-le` | Less or Equal | Menor o igual que (`<=`) |
| `-gt` | Greater Than | Mayor que (`>`) |
| `-ge` | Greater or Equal | Mayor o igual que (`>=`) |

#### Comprobaciones sobre Archivos y Rutas (File Tests)
| Flag | Prueba |
| :--- | :--- |
| `-e $ruta` | La ruta existe en el sistema de ficheros |
| `-f $ruta` | Existe y es un archivo regular (no directorio ni dispositivo) |
| `-d $ruta` | Existe y es un directorio |
| `-r $ruta` | Tiene permiso de lectura para el usuario actual |
| `-w $ruta` | Tiene permiso de escritura para el usuario actual |
| `-x $ruta` | Tiene permiso de ejecución para el usuario actual |
| `-s $ruta` | Existe y su tamaño es superior a 0 bytes |
| `-L $ruta` o `-h` | Es un enlace simbólico |
| `-b $ruta` | Es un archivo especial de dispositivo de bloques (ej: `/dev/sda`) |
| `-c $ruta` | Es un archivo especial de dispositivo de caracteres (ej: `/dev/tty`) |
| `f1 -nt f2` | *Newer than*: `f1` ha sido modificado más recientemente que `f2` |
| `f1 -ot f2` | *Older than*: `f1` es más antiguo que `f2` |

### 6.3 Operadores Lógicos
- En el test clásico `[ ... ]`: `!` (NOT), `-a` (AND), `-o` (OR).
- En el test avanzado `[[ ... ]]`: `!` (NOT), `&&` (AND), `||` (OR).

```bash
# Comprobación de privilegios de superusuario (root)
if [ "$EUID" -ne 0 ]; then
    echo "ERROR: Este script debe ejecutarse como root (sudo)." >&2
    exit 1
fi
```

### 6.4 Sentencia de Selección Múltiple `case`

Ideal para validar menús de texto, opciones pasadas por CLI o analizar patrones:

```bash
#!/bin/bash
case "$1" in
    start|iniciar)
        echo "Iniciando servicio..."
        systemctl start mi-servicio
        ;;
    stop|parar)
        echo "Deteniendo servicio..."
        systemctl stop mi-servicio
        ;;
    restart|reiniciar)
        echo "Reiniciando servicio..."
        systemctl restart mi-servicio
        ;;
    status)
        systemctl status mi-servicio
        ;;
    *)
        echo "Uso: $0 {start|stop|restart|status}"
        exit 2
        ;;
esac
```

---

## 7. Estructuras Repetitivas (Bucles)

### 7.1 El Bucle `for`

#### Iteración sobre listas y comandos
```bash
for SERVICIO in sshd nginx cron; do
    systemctl is-active --quiet "$SERVICIO" && echo "$SERVICIO: ACTIVO" || echo "$SERVICIO: INACTIVO"
done
```

#### Iteración por rangos y números
```bash
# Con expansión de llaves:
for i in {1..5}; do
    echo "Intento $i de 5"
done

# Estilo C:
for (( i=0; i<10; i+=2 )); do
    echo "Paso par: $i"
done
```

### 7.2 El Bucle `while` y Lectura Segura de Archivos

El bucle `while` se ejecuta mientras la condición devuelva código de éxito (0). Es la herramienta idónea para procesar archivos de configuración o logs línea por línea:

```bash
#!/bin/bash
# Procesar archivo CSV/delimitado de usuarios (formato: usuario:uid:grupo)
FICHERO="/root/usuarios.txt"

if [ ! -f "$FICHERO" ]; then
    echo "El archivo $FICHERO no existe."
    exit 1
fi

while IFS=":" read -r USUARIO UID GRUPO; do
    echo "Creando usuario: $USUARIO (UID: $UID, Grupo: $GRUPO)"
    # useradd -u "$UID" -g "$GRUPO" -m "$USUARIO"
done < "$FICHERO"
```

> [!IMPORTANT] Por qué usar `IFS= read -r linea`
> - `IFS=`: Evita que se recorten los espacios o tabuladores al inicio o al final de cada línea.
> - `-r`: Evita que las barras invertidas (`\`) sean interpretadas como caracteres de escape, preservando las rutas íntegras.

### 7.3 El Bucle `until`
Se ejecuta **hasta que** la condición se vuelva verdadera (mientras sea falsa):

```bash
PUERTO=80
until nc -z localhost $PUERTO; do
    echo "Esperando a que el servidor web en el puerto $PUERTO responda..."
    sleep 2
done
echo "¡Servidor web operativo!"
```

### 7.4 Control de Flujo: `break` y `continue`
- `break`: Rompe y sale inmediatamente del bucle actual.
- `continue`: Omite las instrucciones restantes de la iteración actual y pasa a la siguiente.

---

## 8. Menús Interactivos con `select`

La instrucción `select` genera de forma automática un menú numérico con las opciones especificadas, leyendo la elección del usuario en la variable `$REPLY`:

```bash
#!/bin/bash
PS3="Seleccione una opción de mantenimiento [1-4]: "

select OPCION in "Copias de seguridad" "Espacio en disco" "Usuarios conectados" "Salir"; do
    case "$REPLY" in
        1)
            echo "Ejecutando backup en /backup..."
            tar -czf /backup/etc-$(date +%F).tar.gz /etc 2>/dev/null
            ;;
        2)
            df -h /
            ;;
        3)
            who
            ;;
        4)
            echo "Saliendo del asistente..."
            break
            ;;
        *)
            echo "Opción no válida. Inténtelo de nuevo."
            ;;
    esac
done
```

---

## 9. Modularidad y Funciones en Bash

Las funciones permiten estructurar el código, evitar duplicidades y mejorar la mantenibilidad de los scripts administrativos.

```bash
#!/bin/bash

# Declaración de la función
verificar_paquete() {
    local PAQUETE="$1"  # Variable de ámbito exclusivamente local

    if dpkg -s "$PAQUETE" >/dev/null 2>&1; then
        echo "[OK] El paquete '$PAQUETE' está instalado."
        return 0
    else
        echo "[FALLO] El paquete '$PAQUETE' NO está instalado."
        return 1
    fi
}

# Invocación pasando parámetros
verificar_paquete "curl"
if [ $? -ne 0 ]; then
    apt update && apt install -y curl
fi
```

---

## 10. Interfaces de Texto (TUI) con `dialog`

La herramienta `dialog` permite diseñar interfaces semigráficas basadas en consola (ncurses) para dotar a los scripts de menús profesionales en servidores sin entorno gráfico:

```bash
sudo apt install -y dialog
```

### 10.1 Cajas Básicas de Información y Confirmación

```bash
# Caja de mensaje informativa:
dialog --title "Mantenimiento" --msgbox "La base de datos ha sido optimizada." 8 45

# Caja de confirmación (Si/No):
if dialog --title "Reinicio" --yesno "¿Desea reiniciar el servidor ahora?" 7 50; then
    reboot
else
    echo "Reinicio cancelado."
fi
```

### 10.2 Entrada de Datos (`--inputbox`) y Selección de Menú (`--menu`)

```bash
#!/bin/bash
# Captura de salida redirigiendo el descriptor 2 (stderr)
USUARIO=$(dialog --title "Alta de Usuario" --inputbox "Introduce el nuevo login:" 8 40 2>&1 >/dev/tty)

if [ -n "$USUARIO" ]; then
    ACCION=$(dialog --title "Rol del Usuario" --menu "Selecciona el perfil:" 12 40 3 \
        1 "Administrador" \
        2 "Operador" \
        3 "Invitado" 2>&1 >/dev/tty)

    clear
    echo "Creando usuario $USUARIO con rol $ACCION..."
fi
```

---

## 📋 Resumen Rápido de Buenas Prácticas en Bash

1. **Shebang explícito**: Utiliza siempre `#!/bin/bash`.
2. **Control estricto de errores**: Emplea `set -euo pipefail` en scripts de producción críticos.
3. **Comillas en variables**: Siempre que una variable pueda contener espacios o nombres de archivos, enciérrala entre comillas dobles: `"$FICHERO"`.
4. **Comprobación de privilegios**: Verifica si el script requiere privilegios mediante `if [ "$EUID" -ne 0 ]`.
5. **Códigos de salida significativos**: Termina con `exit 0` en ejecuciones exitosas y códigos `1..255` documentados en caso de error.
