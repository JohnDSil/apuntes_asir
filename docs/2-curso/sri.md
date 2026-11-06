# Servicios de Red e Internet (SRI)

Configuración, diagnóstico y securización de servicios de red en entornos Linux/Windows (DNS, DHCP, Web, FTP, SSH).

---

## 🌍 Servidor DNS (Bind9)

- Archivos principales en `/etc/bind/`:
  - `named.conf.options`: Reenviadores (forwarders) y opciones de consulta.
  - `named.conf.local`: Declaración de zonas directas e inversas.
  - `/etc/bind/db.midominio.lan`: Registros de zona (SOA, NS, A, CNAME, MX).

```text
; Comprobación de sintaxis de configuración y zonas
named-checkconf
named-checkzone midominio.lan /etc/bind/db.midominio.lan
```

---

## 📡 Servidor DHCP (Kea / ISC-DHCP)

- Configuración de subred, rangos de asignación dinámica y reservas por dirección MAC.
- Verificación del estado del servicio:
  ```bash
  systemctl status isc-dhcp-server
  journalctl -u isc-dhcp-server -f
  ```
