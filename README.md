# 🐳 WordPress Docker Project

---

## 🚀 Start the project

```bash
docker-compose up -d
```

- WordPress: [http://localhost:8080](http://localhost:8080)
- phpMyAdmin: [http://localhost:8081](http://localhost:8081)

---

## ⏹️ Stop the project (keep data)

```bash
docker-compose stop
```

> Data and WordPress files are preserved and can be resumed normally.

---

## ▶️ Restart after stopping

```bash
docker-compose start
```

---

## 🔄 Restart the project

```bash
docker-compose restart
```

---

## 🗑️ Remove the project completely

> ⚠️ **Warning:** All database data and files will be permanently deleted!

**Step 1:** Stop and remove containers + database volume
```bash
docker-compose down -v
```

**Step 2:** Delete the project folder on your machine (Windows)
```bash
cd ..
rd /s /q your_project_folder_name
```

---

## 📁 Cấu trúc thư mục

```
your_project_folder_name/
├── docker-compose.yml
├── .env
├── README.md
└── wordpress/          ← All WordPress files (theme, plugin, upload...)
```

---

## ⚙️ Configuration (.env)

| Variable | Description | Default |
|----------|-------------|---------|
| `WP_PORT` | WordPress access port | `8080` |
| `PMA_PORT` | phpMyAdmin access port | `8081` |
| `DB_NAME` | Database name | `wordpress` |
| `DB_USER` | Database username | `root` |
| `DB_PASSWORD` | Database password | `password` |
| `DB_ROOT_PASSWORD` | MariaDB root password | `password` |
