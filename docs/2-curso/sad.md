# Seguridad y Alta Disponibilidad (SAD)

Políticas de seguridad perimetral, cortafuegos, túneles seguros VPN, certificados SSL/TLS y copias de seguridad.

---

## 🛡️ Cortafuegos UFW / iptables

```bash
# Estado y activación de UFW
sudo ufw status verbose
sudo ufw enable

# Reglas por defecto
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Permitir SSH, HTTP y HTTPS
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

---

## 🔒 Certificados SSL con Let's Encrypt / Certbot

```bash
# Obtener e instalar certificado automático para Nginx
sudo certbot --nginx -d midominio.com -d www.midominio.com

# Simulación de renovación automática
sudo certbot renew --dry-run
```
