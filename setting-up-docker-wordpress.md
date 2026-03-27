# 🐳 WordPress Docker Project

> Run WordPress locally with Docker, MariaDB, and phpMyAdmin — no manual installation required.

---

## 📋 Prerequisites

Download and install **[Docker Desktop](https://www.docker.com/products/docker-desktop/)** for your operating system before getting started.

---

## 📁 Project Structure

```
your_project_folder/
├── docker-compose.yml
├── .env
└── README.md
```

---

## ⚙️ Step 1 — Configure Environment Variables

Create a `.env` file in your project root:

```env
# Database Configuration
DB_ROOT_PASSWORD=password
DB_NAME=wordpress
DB_USER=root
DB_PASSWORD=password

# WordPress Configuration
WP_PORT=8080

# phpMyAdmin Configuration
PMA_PORT=8081
```

> ⚠️ **Important:** Change the default passwords before deploying to production!

---

## 🐋 Step 2 — Set Up Docker Compose

Create a `docker-compose.yml` file:

```yaml
services:
  db:
    image: mariadb:latest
    container_name: wordpress_db
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASSWORD}
      MYSQL_DATABASE: ${DB_NAME}
      MYSQL_USER: ${DB_USER}
      MYSQL_PASSWORD: ${DB_PASSWORD}
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - wordpress_network

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    container_name: wordpress_site
    restart: unless-stopped
    ports:
      - "${WP_PORT}:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_NAME: ${DB_NAME}
      WORDPRESS_DB_USER: ${DB_USER}
      WORDPRESS_DB_PASSWORD: ${DB_PASSWORD}
    volumes:
      - wp_data:/var/www/html
    networks:
      - wordpress_network

  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin:latest
    container_name: wordpress_phpmyadmin
    restart: unless-stopped
    ports:
      - "${PMA_PORT}:80"
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
      MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASSWORD}
    networks:
      - wordpress_network

volumes:
  db_data:
  wp_data:

networks:
  wordpress_network:
    driver: bridge
```

---

## 🚀 Step 3 — Launch Your WordPress Site

Navigate to your project folder in the terminal and run:

```bash
docker-compose up -d
```

That's it! Your WordPress environment is now up and running.

---

## 🌐 Access Your Services

| Service | URL |
|---------|-----|
| WordPress | [http://localhost:8080](http://localhost:8080) |
| phpMyAdmin | [http://localhost:8081](http://localhost:8081) |

**Website files location (Windows):**
```
\\wsl.localhost\docker-desktop\tmp\docker-desktop-root\var\lib\docker\volumes\YOUR_WEBSITE_wp_data\_data\
```

---

## ⏹️ Stop the Project (keep data)

```bash
docker-compose stop
```

> Data and WordPress files are preserved and can be resumed normally.

---

## ▶️ Restart After Stopping

```bash
docker-compose start
```

---

## 🔄 Restart the Project

```bash
docker-compose restart
```

---

## 🗑️ Remove Containers & Data

> ⚠️ **Warning:** This will permanently delete all containers and data!

```bash
docker-compose down --volumes
```

---

## 📤 Fix Max Upload File Size

If you see the error *"The uploaded file exceeds the upload_max_filesize directive in php.ini"* when uploading a plugin or theme, open your `.htaccess` file and add:

```
php_value upload_max_filesize 500M
php_value post_max_size 500M
```

---

## 📦 What's Included

| Service | Description |
|---------|-------------|
| **MariaDB** | Robust database server for WordPress |
| **WordPress** | Latest version of WordPress, ready to configure |
| **phpMyAdmin** | Web interface for managing your database |

All services are connected through a dedicated Docker network and use persistent volumes to safely store your data.

---

## 🔗 Reference

Guide by [Raddy](https://raddy.dev/blog/how-to-run-wordpress-locally-with-docker-easy-setup-guide-mariadb-phpmyadmin/)