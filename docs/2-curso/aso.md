# Administración de Sistemas Operativos (ASO)

Automatización de tareas mediante Bash scripting, gestión avanzada de almacenamiento, usuarios y monitorización del sistema.

---

## 📜 Bash Scripting

### Estructura de comprobaciones y bucles
```bash
#!/bin/bash
# Comprobar si un servicio está activo
SERVICIO="nginx"

if systemctl is-active --quiet "$SERVICIO"; then
    echo "El servicio $SERVICIO está funcionando correctamente."
else
    echo "ALERTA: El servicio $SERVICIO está caído. Reiniciando..."
    systemctl restart "$SERVICIO"
fi
```

### Automatización con Cron
```bash
# Editar crontab del usuario actual
crontab -e

# Formato: min hora dia mes dia_semana comando
# Ejecutar backup todos los días a las 02:30 AM:
30 2 * * * /usr/local/bin/backup.sh > /var/log/backup.log 2>&1
```
