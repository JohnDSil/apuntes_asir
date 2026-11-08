# 🚀 Plataforma de Estudio ASIR
*(Administración de Sistemas Informáticos en Red)*

Bienvenido al repositorio central de apuntes, laboratorios prácticos y preparación de exámenes para el ciclo formativo de grado superior de **ASIR**. Diseñado para el estudio eficiente con chuletas de comandos, casos prácticos reales y autoevaluación.

---

## 🎯 Selector Rápido de Módulos (2º Curso)

<div class="grid cards" markdown>

-   :material-shield-lock:{ .lg .middle } **Seguridad y Alta Disponibilidad (SAD)**

    ---

    Criptografía, control de acceso, modelo CIDAN, dimensionado de SAIs, copias 3-2-1, análisis forense digital y marco legal RGPD / AEPD.

    <span class="badge badge-exam">5 Unidades</span>
    <span class="badge badge-practice">Prácticas + Ley</span>

    [:octicons-arrow-right-24: Ir al módulo de SAD](2-curso/sad/index.md)

-   :material-console:{ .lg .middle } **Administración de Sistemas Operativos (ASO)**

    ---

    Scripting en Bash desde cero a avanzado, automatización con menús gráficos Zenity y programación de tareas con Cron, Anacron y Systemd Timers.

    <span class="badge badge-exam">3 Unidades</span>
    <span class="badge badge-practice">Scripts Listos</span>

    [:octicons-arrow-right-24: Ir al módulo de ASO](2-curso/aso/index.md)

-   :material-database:{ .lg .middle } **Gestión de Bases de Datos Avanzada (ASGBD)**

    ---

    Contenedores Docker para SGBD, Oracle Database, DDL, DML, transacciones ACID, JOINs complejos, subconsultas y procedimientos PL/SQL.

    <span class="badge badge-exam">7 Unidades</span>
    <span class="badge badge-theory">SQL + Oracle</span>

    [:octicons-arrow-right-24: Ir al módulo de ASGBD](2-curso/asgbd/index.md)

-   :material-web:{ .lg .middle } **Implantación de Aplicaciones Web (IAW)**

    ---

    Arquitectura cliente-servidor, protocolo HTTP a fondo, métodos GET/POST, programación backend PHP y despliegue multicontenedor con Docker.

    <span class="badge badge-exam">4 Unidades</span>
    <span class="badge badge-practice">Docker + PHP</span>

    [:octicons-arrow-right-24: Ir al módulo de IAW](2-curso/iaw/index.md)

-   :material-infinity:{ .lg .middle } **Herramientas DevOps**

    ---

    Cultura DevOps, control de versiones con Git y GitHub, flujo de ramas, documentación Markdown y monitorización de servidores con Prometheus y Grafana.

    <span class="badge badge-exam">3 Unidades</span>
    <span class="badge badge-key">CI/CD + Git</span>

    [:octicons-arrow-right-24: Ir al módulo de DevOps](2-curso/devops/index.md)

-   :material-server-network:{ .lg .middle } **Servicios de Red e Internet (SRI)**

    ---

    Servicios de infraestructura: DNS (Bind9), asignación dinámica DHCP, servidores web Apache/Nginx, FTP seguro y acceso remoto SSH.

    <span class="badge badge-theory">En preparación</span>

    [:octicons-arrow-right-24: Ver temario SRI](2-curso/sri.md)

</div>

---

## 📚 Módulos de 1º Curso (Fundamentos)

<div class="grid cards" markdown>

-   :material-linux: **Implantación de Sistemas Operativos (ISO)**
    
    Instalación y configuración de Linux (Debian/Ubuntu) y Windows Server, particionado y gestión de usuarios.
    
    [:octicons-arrow-right-24: Ver apuntes](1-curso/iso.md)

-   :material-lan: **Planificación y Adm. de Redes (PAR)**
    
    Subnetting VLSM IPv4/IPv6, modelo OSI/TCP-IP, conmutación Ethernet, VLANs y enrutamiento estático/dinámico.
    
    [:octicons-arrow-right-24: Ver apuntes](1-curso/redes.md)

-   :material-database-outline: **Gestión de Bases de Datos (GBD)**
    
    Modelado conceptual Entidad-Relación, normalización (1FN a 3FN), SQL relacional y MySQL/MariaDB.
    
    [:octicons-arrow-right-24: Ver apuntes](1-curso/bbdd.md)

-   :material-memory: **Fundamentos de Hardware (FH)**
    
    Arquitectura de ordenadores, buses, microprocesadores, memoria RAM, fuentes de alimentación y BIOS/UEFI.
    
    [:octicons-arrow-right-24: Ver apuntes](1-curso/hardware.md)

-   :material-code-tags: **Lenguajes de Marcas (LMSGI)**
    
    Sintaxis HTML5, hojas de estilo CSS3, validación XML con DTD y XSD, y consultas XPath.
    
    [:octicons-arrow-right-24: Ver apuntes](1-curso/marcas.md)

</div>

---

## 💡 Cómo aprovechar esta web para estudiar

=== "🔍 Búsqueda Instantánea"
    Pulsa la tecla <kbd>S</kbd> o <kbd>/</kbd> en cualquier momento para abrir el buscador global. Escribe cualquier comando (ej: `journalctl`, `systemctl`, `chmod`, `iptables`) o concepto (`CIDAN`, `ACID`, `SAI`) para saltar directamente a la explicación con ejemplos.

=== "📋 Código y Comandos Copiables"
    Todos los scripts y líneas de terminal cuentan con un botón de copiado rápido en la esquina superior derecha. Además, las pestañas de comandos recuerdan tu selección: si eliges **Linux** o **Windows** en una pestaña, se mantendrá en todo el tema.

=== "🎓 Enfoque a Examen"
    Busca los bloques destacados con etiquetas:
    
    - <span class="badge badge-exam">Pregunta de Examen</span>: Conceptos típicos de preguntas teóricas y supuestos prácticos.
    - `[!WARNING] Trampa típica`: Errores frecuentes cometidos en prácticas de laboratorio o exámenes.
    - `???+ question`: Bloques de autoevaluación desplegables con soluciones explicadas paso a paso.
