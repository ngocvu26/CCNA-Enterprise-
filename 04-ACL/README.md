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

### A. Layer 2 Isolation (Access Switches)
- Phân lập lưu lượng thành 3 Broadcast Domain độc lập:
  - `SW-INSIDE`: Toàn bộ cổng hoạt động đặt trong `VLAN 10` (xem cấu hình tại [SW-INSIDE.txt](configs/SW-INSIDE.txt))
  - `SW-DMZ`: Toàn bộ cổng hoạt động đặt trong `VLAN 50` (xem cấu hình tại [SW-DMZ.txt](configs/SW-DMZ.txt))
  - `SW-BRANCH`: Toàn bộ cổng hoạt động đặt trong `VLAN 100` (xem cấu hình tại [SW-BRANCH.txt](configs/SW-BRANCH.txt))
- Đưa toàn bộ cổng không dùng vào VLAN 99

### B. Firewall Inbound Policy (Cisco ASA)
- Toàn bộ cấu hình thiết bị: xem chi tiết tại [Cisco-ASA.txt](configs/Cisco-ASA.txt)

### C. Edge Router Configuration & Standard ACL Rule (Cisco IOS)
- Toàn bộ cấu hình thiết bị: xem chi tiết tại [EDGE-Router.txt](configs/EDGE-Router.txt)
---

## 4. Test Matrix & Verification

| Mã kiểm thử | Máy gửi (Source) | Máy nhận (Target) | Kết quả |
| :---: | :--- | :--- | :---: |
| **TC-01** | PC-Admin (`.10.10`) | Branch-Srv (`.100.100`) | **PERMIT** |
| **TC-02** | Branch-Srv (`.100.100`) | Web-Server (`.50.80`) | **PERMIT** |
| **TC-03** | PC-Admin (`.10.10`) / PC-Staff (`.10.20`) | Web-Server (`.50.80`) | **PERMIT** |
| **TC-04** | PC-Staff (`.10.20`) | Branch-Srv (`.100.100`) | **DENY** |
| **TC-05** | PC-Staff (`.10.20`) | Web-Server (`.50.80`) | **PERMIT** |
