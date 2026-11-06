# Implantación de Sistemas Operativos (ISO)

Apuntes y comandos fundamentales sobre instalación, configuración y administración básica de sistemas operativos GNU/Linux y Microsoft Windows Server.

---

## 🐧 Comandos básicos de GNU/Linux

### Navegación y permisos
```bash
# Permisos y propietarios
chmod 755 script.sh
chown usuario:grupo archivo.txt

# Espacio en disco y memoria
df -h
free -m
lsblk
```

### Gestión de procesos
```bash
ps aux | grep nginx
top
htop
kill -9 <PID>
```

---

## 🪟 Windows Server

- **Active Directory Domain Services (AD DS):** Instalación de rol de controlador de dominio y creación de Unidades Organizativas (OU).
- **Directivas de Grupo (GPO):** Configuración de políticas de seguridad para usuarios y equipos de dominio.
- **PowerShell básico:**
  ```powershell
  Get-Service | Where-Object {$_.Status -eq "Running"}
  Get-ADUser -Filter *
  ```
