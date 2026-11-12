# 🌐 CCNA Enterprise Networking Portfolio

Tập hợp toàn bộ các bài lab thực hành từ cơ bản đến nâng cao theo chuẩn **CCNA (200-301)** và dự án tổng hợp thiết kế hạ tầng mạng doanh nghiệp 3 lớp.

---

## Dự án Tổng hợp  
* **[FINAL-ENTERPRISE-CAMPUS-3-TIER](./FINAL-ENTERPRISE-CAMPUS-3-TIER)** — **Thiết kế & Mô phỏng Hạ tầng Mạng Doanh nghiệp 3 Lớp (Core – Distribution – Access)**
  * **Kiến trúc & Công nghệ:** Mô hình 3-Tier Campus, VLAN & Trunking, RSTP/PVST+, EtherChannel (LACP), Dự phòng Gateway với HSRP, Định tuyến động Multi-Area OSPF, NAT/PAT, Extended ACLs và Bảo mật Layer 2.
  * **Tài liệu đi kèm:** Sơ đồ kiến trúc (Topology), file cấu hình thiết bị (`configurations/`) và kết quả kiểm thử hệ thống (`verifications/`).

---

##  Danh sách các Module Lab Thực hành

| STT | Module / Thư mục | Nội dung kỹ thuật trọng tâm | Trạng thái |
| :---: | :--- | :--- | :---: |
| 01 | **[01-enterprise-switching](./01-enterprise-switching)** | VLAN, Trunking (802.1Q), Inter-VLAN Routing, STP / RSTP | ✅ Hoàn thành |
| 02 | **[02-vlsm-ip-planing](./02-vlsm-ip-planing)** | Quy hoạch địa chỉ IP, chia mạng con biến đổi chiều dài (VLSM) | ✅ Hoàn thành |
| 03 | **[03-enterprise-routing-ospf](./03-enterprise-routing-ospf)** | Định tuyến động OSPF (Single & Multi-Area), DR/BDR, Route Summarization | ✅ Hoàn thành |
| 04 | **[04-ACL](./04-ACL)** | Kiểm soát truy cập mạng với Standard & Extended Access Control Lists | ✅ Hoàn thành |
| 05 | **[05-security-layer2-&-dhcp](./05-security-layer2-&-dhcp)** | Triển khai DHCP Server/Relay, Port Security, DHCP Snooping, DAI | ✅ Hoàn thành |
| 06 | **[06-nat](./06-nat)** | Cấu hình Static NAT, Dynamic NAT và PAT (NAT Overload) kết nối Internet | ✅ Hoàn thành |

---

## 🛠️ Công cụ & Môi trường sử dụng
* **Mô phỏng mạng:** Cisco Packet Tracer / GNS3
* **Thiết bị cấu hình:** Cisco IOS Router & Catalyst Switch (Layer 2 / Layer 3)
