# Đề 3: Website quảng bá sản phẩm (WordPress)

- **Môn học**: Triển khai và Quản trị Hệ thống Phần mềm
- **Sinh viên**: Nguyễn Hà Nam
- **Mã sinh viên**: DTC245010061
- **Email**: dtc245010061@ictu.edu.vn

---

## 1. Giới thiệu kiến trúc hệ thống

Hệ thống được đóng gói bằng Docker Compose bao gồm các dịch vụ[cite: 1]:
- **Ứng dụng & CSDL**: WordPress (Apache + PHP 8.3), MySQL 8.0, phpMyAdmin[cite: 1].
- **Reverse Proxy**: Nginx xử lý HTTPS tự ký, chuyển hướng HTTP sang HTTPS, bổ sung Security Headers[cite: 1].
- **Giám sát**: Prometheus + Grafana (Node Exporter, cAdvisor, Blackbox Exporter, MySQL Exporter)[cite: 1].
- **Quản lý log**: Grafana Loki + Promtail thu thập log container thời gian thực[cite: 1].
- **Mạng cô lập**: Tách biệt app-network và monitoring[cite: 1].

---

## 2. Danh sách dịch vụ và địa chỉ truy cập

| Dịch vụ | Địa chỉ | Mô tả |
|---|---|---|
| **Website (WordPress)** | https://192.168.184.130 | Web chính qua Nginx Reverse Proxy[cite: 1] |
| **Quản trị CSDL (phpMyAdmin)** | http://192.168.184.130:8082 | Quản lý database WordPress[cite: 1] |
| **Prometheus** | http://192.168.184.130:9090 | Targets và metrics giám sát[cite: 1] |
| **Grafana** | http://192.168.184.130:3000 | Dashboard giám sát (Monitoring Lab)[cite: 1] |

---

## 3. Truy vấn log mẫu với LogQL

- Toàn bộ log WordPress: `{container="wordpress"}`[cite: 1]
- Log lỗi 404 qua Nginx: `{container="nginx"} |= "404"`[cite: 1]
- Cảnh báo hoặc lỗi: `{container=~"wordpress|mysql"} |~ "(?i)(error|warn)"`[cite: 1]

---

## 4. Lệnh quản trị

- Dừng hệ thống (giữ dữ liệu): `docker compose down`[cite: 1]
- Dừng và xóa dữ liệu: `docker compose down -v`[cite: 1]
