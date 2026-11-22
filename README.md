# wazuh-siem-homelab
Wazuh SIEM Home Lab: Custom Detection Rules &amp; SQL Injection Simulation on XAMPP
## 🛡️ Mô phỏng Tấn công Web (SQL Injection) & Phát hiện trên Wazuh SIEM

### 1. Môi trường & Luồng thu thập Log
- **Wazuh Manager:** Ubuntu Linux (`192.168.1.5`)
- **Wazuh Agent:** Windows (`Windows-LOQ`)
- **Web Server mục tiêu:** XAMPP Apache
- **Nguồn log giám sát:** `C:\xamppp\apache\logs\access.log` (cấu hình `<log_format>apache</log_format>` trong `ossec.conf`)

### 2. Thực hiện tấn công mô phỏng (Attack Simulation)
Gửi request chứa payload SQL Injection trực tiếp vào Web Server cục bộ:
```http
GET /index.php?id=1' OR '1'='1 HTTP/1.1
```

File `access.log` trên XAMPP ghi nhận lại request tấn công (đã được trình duyệt URL-encode):
![Apache Access Log](<Screenshot 2026-11-22 230339.png>)

### 3. Kết quả phát hiện trên Wazuh Dashboard (Detection)
Wazuh Agent tự động thu thập dòng log mới và gửi về Wazuh Manager. Custom Rule **ID `100005` (Level 10)** đã kích hoạt cảnh báo thời gian thực trên giao diện **Threat Hunting → Events**:

![Wazuh Dashboard Alerts](<Screenshot 2026-11-22 230750.png>)

**Chi tiết cảnh báo (Alert Details):**
Hiển thị đầy đủ thông tin về Agent (`Windows-LOQ`), mức độ nghiêm trọng (`rule.level: 10`), mô tả cảnh báo và chuỗi payload gốc (`full_log`):

![Wazuh Alert Detail](<Screenshot 2026-11-22 230946.png>)

