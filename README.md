# 🐳 WordPress Docker Project

---

## 🚀 Khởi động dự án

```bash
docker-compose up -d
```

- WordPress: [http://localhost:8080](http://localhost:8080)
- phpMyAdmin: [http://localhost:8081](http://localhost:8081)

---

## ⏹️ Dừng dự án (giữ lại data)

```bash
docker-compose stop
```

> Data và file WordPress vẫn được giữ nguyên, chạy lại bình thường.

---

## ▶️ Chạy lại sau khi dừng

```bash
docker-compose start
```

---

## 🔄 Restart dự án

```bash
docker-compose restart
```

---

## 🗑️ Xoá sạch dự án

> ⚠️ **Cảnh báo:** Toàn bộ database và file sẽ bị xoá vĩnh viễn!

**Bước 1:** Dừng và xoá containers + volume database
```bash
docker-compose down -v
```

**Bước 2:** Xoá folder project trên máy (Windows)
```bash
cd ..
rd /s /q tên_folder_project
```

---

## 📁 Cấu trúc thư mục

```
tên_folder_project/
├── docker-compose.yml
├── .env
├── README.md
└── wordpress/          ← Toàn bộ file WordPress (theme, plugin, upload...)
```

---

## ⚙️ Cấu hình (.env)

| Biến | Mô tả | Mặc định |
|------|-------|---------|
| `WP_PORT` | Port truy cập WordPress | `8080` |
| `PMA_PORT` | Port truy cập phpMyAdmin | `8081` |
| `DB_NAME` | Tên database | `wordpress` |
| `DB_USER` | Username database | `root` |
| `DB_PASSWORD` | Password database | `password` |
| `DB_ROOT_PASSWORD` | Password root MariaDB | `password` |
