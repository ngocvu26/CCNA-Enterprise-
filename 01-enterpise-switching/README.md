## A - Overview

* **Tên Lab:** Xây dựng mô hình mạng hai lớp (`Access Layer` - `Distribution Layer`)
* **Mục tiêu:** Loại bỏ vòng lặp Layer 2, tối ưu hóa cơ chế dự phòng và cân bằng tải qua liên kết gom

---

## B - Sơ đồ mạng

* **Link sơ đồ:** 

![Sơ đồ mạng](docs/topology.png)

---

## C - Các công nghệ & Quyết định thiết kế

1. **Spanning Tree Protocol:** Triển khai Rapid-PVST+ kết hợp cân bằng tải VLAN
2. **EtherChannel & Hash Algorithm:** Gom trunk giữa 2 MLS bằng LACP (`mode active`) & phân tải lưu lượng `src-dst-ip` đa dạng luồng
3. **Access Layer Hardening:** Kích hoạt `PortFast` tránh thời gian chờ, `BPDU Guard` tránh có SW lạ cắm vào
4. **Uplink Strategy:** Lựa chọn mô hình 2 dây (Mỗi dây 1 MLS) nhằm tối ưu tài nguyên và 4 dây đối với enterprise lớn (Stacking)

---

## D - Kiểm thử và xác thực

### 1. `show spanning-tree vlan 10` & `vlan 20`
* **VLAN 10:**
  ![VLAN 10 Root](docs/VLAN%2010%20-%20Root.png)

* **VLAN 20:**
  ![VLAN 20 Root](docs/VLAN%2020%20-%20Root.png)

### 2. `show etherchannel summary`
![EtherChannel Summary](docs/etherchannel-summary.png)

### 3. `show etherchannel load-balance`
![EtherChannel Load Balance](docs/etherchannel-load-balance.png)

---

## E - Hướng dẫn chạy thử

1. Cisco Packet Tracer 9.0 Windows
2. Tải file `.pkt` trong thư mục `/lab` và chạy
