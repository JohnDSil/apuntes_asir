# 🛡️ Seguridad y Alta Disponibilidad (SAD)

<span class="badge badge-exam">Módulo Completo (5 Unidades)</span>
<span class="badge badge-practice">Prácticas + Legislación</span>

Bienvenido al módulo de **Seguridad y Alta Disponibilidad (SAD)** de 2º de ASIR. Este módulo te enseñará cómo proteger los sistemas de información frente a fallos mecánicos, ataques informáticos y desastres físicos, garantizando además el cumplimiento estricto de las leyes de privacidad vigentes.

---

## 🗺️ Mapa de Aprendizaje del Módulo

Sigue el orden recomendado de las unidades para construir tu conocimiento paso a paso:

```mermaid
flowchart LR
    U1["UT1: Fundamentos\n(CIDAN, Activos, Riesgo)"] --> U2["UT2: Seguridad Física\n(CPD, SAIs, Backups, RAID)"]
    U2 --> U3["UT3: Criptografía y Acceso\n(AES, RSA, PKI, MFA, ACL)"]
    U3 --> U4["UT4: Amenazas y Forense\n(Ingeniería Social, Firewall, Forense)"]
    U4 --> U5["UT5: Marco Legal\n(RGPD, AEPD, IA Agéntica)"]
```

---

## 📚 Unidades Temáticas de Estudio

<div class="grid cards" markdown>

-   <div class="course-header">
        <div class="course-icon">:material-shield-check:</div>
        <div>
            <div class="course-code">Unidad 1</div>
            <div class="course-title">Fundamentos de Seguridad</div>
        </div>
    </div>
    <div class="course-desc">
        Aprende el modelo mental de la seguridad: modelo <strong>CIDAN</strong>, la ecuación del riesgo ($P \times I$), seguridad física vs lógica y activa vs pasiva.
    </div>
    <div class="course-footer">
        <span class="badge badge-theory">Teoría Base</span>
        <a class="course-btn" href="01-fundamentos-seguridad.md">Estudiar Unidad :octicons-arrow-right-24:</a>
    </div>

-   <div class="course-header">
        <div class="course-icon">:material-server:</div>
        <div>
            <div class="course-code">Unidad 2</div>
            <div class="course-title">Seguridad Física, SAI y Backups</div>
        </div>
    </div>
    <div class="course-desc">
        Condiciones de un CPD, cálculo de potencia de un <strong>SAI (VA y W)</strong>, estrategias de copias de seguridad (regla 3-2-1) y tolerancia a fallos con <strong>RAID</strong>.
    </div>
    <div class="course-footer">
        <span class="badge badge-practice">Ejercicios de Cálculo</span>
        <a class="course-btn" href="02-seguridad-fisica-sai-backup.md">Estudiar Unidad :octicons-arrow-right-24:</a>
    </div>

-   <div class="course-header">
        <div class="course-icon">:material-key-chain:</div>
        <div>
            <div class="course-code">Unidad 3</div>
            <div class="course-title">Control de Acceso y Criptografía</div>
        </div>
    </div>
    <div class="course-desc">
        Autenticación robusta (PAM, MFA, biometría), cifrado simétrico (AES), asimétrico (RSA), funciones Hash SHA-256, firma digital, PKI, cifrado de discos (LUKS) y TLS/SSH.
    </div>
    <div class="course-footer">
        <span class="badge badge-key">Criptografía</span>
        <a class="course-btn" href="03-control-acceso-criptografia.md">Estudiar Unidad :octicons-arrow-right-24:</a>
    </div>

-   <div class="course-header">
        <div class="course-icon">:material-bug-outline:</div>
        <div>
            <div class="course-code">Unidad 4</div>
            <div class="course-title">Amenazas, Vulnerabilidades y Forense</div>
        </div>
    </div>
    <div class="course-desc">
        Ingeniería social (phishing, vishing), malware y rootkits, métricas CVE/CVSS, protección perimetral (UFW, IDS/IPS, DMZ) y análisis forense con cadena de custodia.
    </div>
    <div class="course-footer">
        <span class="badge badge-exam">Seguridad Perimetral</span>
        <a class="course-btn" href="04-amenazas-vulnerabilidades-forense.md">Estudiar Unidad :octicons-arrow-right-24:</a>
    </div>

-   <div class="course-header">
        <div class="course-icon">:material-scale-balance:</div>
        <div>
            <div class="course-code">Unidad 5</div>
            <div class="course-title">Marco Legal: RGPD y AEPD</div>
        </div>
    </div>
    <div class="course-desc">
        Normativa de privacidad en España y la UE (RGPD, LOPDGDD), derechos ARCO+POL, videovigilancia, control laboral, canales de denuncia e IA Agéntica según la AEPD.
    </div>
    <div class="course-footer">
        <span class="badge badge-exam">Marco Legal</span>
        <a class="course-btn" href="05-marco-legal-rgpd-aepd.md">Estudiar Unidad :octicons-arrow-right-24:</a>
    </div>

</div>

---

## 💡 Consejos para aprobar el módulo de SAD

1. **No memorices comandos sin entender el objetivo**: En seguridad, cada comando o herramienta responde a uno de los pilares del modelo **CIDAN** (Confidencialidad, Integridad, Disponibilidad, Autenticación, No repudio).
2. **Practica los ejercicios numéricos de la UT2**: El dimensionado de un SAI en Volt-Amperios ($VA$) y el cálculo de espacio útil y discos de tolerancia en matrices RAID (0, 1, 5, 6, 10) entran prácticamente siempre en los exámenes prácticos.
3. **Resuelve los Tests de Autoevaluación**: Al final de cada unidad tienes un bloque desplegable de autoevaluación. Intenta responder a la pregunta mentalmente antes de abrir el desplegable.
