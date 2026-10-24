# Đề 3: Website quảng bá sản phẩm (WordPress)

- **Môn học**: Triển khai và Quản trị Hệ thống Phần mềm
- **Sinh viên**: Nguyễn Hà Nam
- **Mã sinh viên**: DTC245010061
- **Email**: dtc245010061@ictu.edu.vn
- **Cửa hàng**: MECHA STORE – Thế giới Mô hình Lắp ráp Gunpla
- **Sản phẩm chủ lực**: Mô hình lắp ráp IN ERA+ 1/100 Nemesis (Advanced Strategic Revenge Titan)

---

## 1. Giới thiệu sản phẩm quảng bá

![IN ERA+ 1/100 Nemesis](nemesis.png)

Hệ thống phục vụ quảng bá và bán hàng cho các dòng mô hình cơ khí chiến đấu cao cấp:
- **Sản phẩm chính**: **IN ERA+ RT-002 Nemesis (Advanced Strategic Revenge Titan)** - Tỉ lệ 1/100, trang bị khung xương Full Inner Frame đa tầng, cánh phản lực Vengeance Wings cơ chế bung nở đa hướng, và cặp vũ khí Dual Tactical Blade-Pistols.
- **Sản phẩm cùng hệ sinh thái**: IN ERA+ 1/100 Genesis và Combo bộ dụng cụ lắp ráp mô hình chuyên nghiệp.

---

## 2. Kiến trúc hệ thống (12 Dịch vụ Container)

Hệ thống được đóng gói và điều phối 100% bằng Docker Compose:
- **Ứng dụng & CSDL**: WordPress (Apache + PHP 8.3), MySQL 8.0, phpMyAdmin.
- **Reverse Proxy**: Nginx xử lý HTTPS tự ký, chuyển hướng HTTP 301 sang HTTPS, bổ sung Security Headers (HSTS, X-Frame-Options, X-Content-Type-Options...).
- **Giám sát**: Prometheus + Grafana (Node Exporter, cAdvisor, Blackbox Exporter, MySQL Exporter).
- **Quản lý log**: Grafana Loki + Promtail thu thập log container thời gian thực qua Docker Socket.
- **Mạng cô lập**: Tách biệt `app-network` và `monitoring`.

---

## 3. Danh sách dịch vụ và địa chỉ truy cập

| Dịch vụ | Địa chỉ | Mô tả |
|---|---|---|
| **Website chính (WordPress)** | https://192.168.184.130 | Web MECHA STORE qua Nginx Reverse Proxy |
| **Quản trị CSDL (phpMyAdmin)** | http://192.168.184.130:8082 | Quản lý database WordPress |
| **Prometheus** | http://192.168.184.130:9090 | Targets và metrics giám sát |
| **Grafana** | http://192.168.184.130:3000 | Dashboard giám sát (Monitoring Lab) |

---

## 4. Truy vấn log mẫu với LogQL (Grafana Explore)

- Toàn bộ log WordPress: `{container="wordpress"}`
- Log lỗi 404 qua Nginx: `{container="nginx"} |= "404"`
- Cảnh báo hoặc lỗi hệ thống: `{container=~"wordpress|mysql"} |~ "(?i)(error|warn)"`
- Thống kê log theo container: `sum by (container) (count_over_time({container=~".+"}[5m]))`

---

## 5. Lệnh quản trị hệ thống

- Khởi động hệ thống: `docker compose up -d`
- Kiểm tra trạng thái: `docker compose ps`
- Dừng hệ thống (giữ dữ liệu): `docker compose down`
- Dừng và xóa dữ liệu: `docker compose down -v`
