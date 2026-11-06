# Implantación de Aplicaciones Web (IAW)

Despliegue de arquitecturas web LAMP/LEMP, contenedores Docker, gestión de CMS (WordPress, Nextcloud) y servidores de aplicaciones PHP.

---

## 🐳 Docker y Docker Compose

### Despliegue de un stack WordPress + MySQL
```yaml
version: '3.8'

services:
  db:
    image: mariadb:10.6
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wp_user
      MYSQL_PASSWORD: wppassword
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    restart: always
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD: wppassword
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wp_data:/var/www/html

volumes:
  db_data:
  wp_data:
```

### Comandos frecuentes
```bash
docker compose up -d    # Levantar los contenedores en segundo plano
docker compose ps       # Listar contenedores del stack
docker compose down     # Parar y destruir contenedores
```
