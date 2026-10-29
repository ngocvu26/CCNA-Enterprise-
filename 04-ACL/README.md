# Enterprise Access Control Lab: ASA Inbound Policies & Router Placement Rules

## 1. Overview
Dự án tập trung triển khai và kiểm chứng các quy tắc kiểm soát truy cập (Access Control Lists - ACLs) trong kiến trúc mạng phân vùng:
- **Extended ACL trên Firewall (Cisco ASA):** Kiểm soát lưu lượng chiều vào (Inbound) từ mạng bên ngoài tới máy chủ Web DMZ trực tiếp theo địa chỉ IP và cổng dịch vụ
- **Standard ACL trên Router biên (Cisco IOS):** Áp dụng nguyên tắc vị trí (Placement Rule) cho bộ lọc Standard ACL, chặn một trạm làm việc cụ thể truy cập máy chủ chi nhánh mà không gây ảnh hưởng tới các luồng dữ liệu khác
- **Hạ tầng Layer 2 & L3:** Phân tách Broadcast Domain qua VLAN độc lập và thiết lập các tuyến định tuyến tĩnh kết nối hai chiều

---

## 2. Network Topology

![Enterprise Network Topology](docs/ACL.png)

---

## 3. Configuration Breakdown

- Phân lập lưu lượng thành 3 Broadcast Domain độc lập:
  - `SW-INSIDE`: Toàn bộ cổng hoạt động đặt trong `VLAN 10`
  - `SW-DMZ`: Toàn bộ cổng hoạt động đặt trong `VLAN 50`
  - `SW-BRANCH`: Toàn bộ cổng hoạt động đặt trong `VLAN 100`
- Đưa toàn bộ cổng không dùng vào VLAN 99

---

## 4. Test Matrix & Verification

| Mã kiểm thử | Máy gửi (Source) | Máy nhận (Target) | Giao thức / Port | Kết quả | Bản chất kỹ thuật & Cơ chế đối soát |
| :---: | :--- | :--- | :---: | :---: | :--- |
| **TC-01** | PC-Admin (`.10.10`) | Branch-Srv (`.100.100`) | TCP / UDP | **PERMIT** | Chiều đi từ Security 100 sang 0 được mở tự động; chiều về khớp phiên trong State Table của ASA |
| **TC-02** | Branch-Srv (`.100.100`) | Web-Server (`.50.80`) | TCP 80, 443 | **PERMIT** | Khớp chính xác Inbound ACL gắn tại cổng Outside của ASA |
| **TC-03** | PC-Admin (`.10.10`) / PC-Staff (`.10.20`) | Web-Server (`.50.80`) | TCP 80, 443 | **PERMIT** | Chiều đi từ Security 100 sang Security 50 được cấp phép tự động theo chính sách mặc định của ASA |
| **TC-04** | PC-Staff (`.10.20`) | Branch-Srv (`.100.100`) | Bất kỳ IP | **DENY** | Vượt qua được ASA, nhưng bị Router drop tại cổng g0/1 Outbound bởi Standard ACL 10 |
| **TC-05** | PC-Staff (`.10.20`) | Web-Server (`.50.80`) | TCP 80 | **PERMIT** | Chứng minh nguyên tắc đặt Standard ACL: Lưu lượng đi sang DMZ không qua cổng g0/1 của Router nên không bị chặn |
