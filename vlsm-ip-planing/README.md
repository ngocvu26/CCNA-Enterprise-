# KẾ HOẠCH QUY HOẠCH ĐỊA CHỈ IP (VLSM IP PLANNING)
**Hệ thống:** Chi nhánh Logistics (Branch Office)  
**Không gian địa chỉ gốc:** `172.16.0.0/20` (Class B)

---

## 1. Phân Tích Mạng Mẹ & Subnet Mask Gốc
* **Mạng gốc:** `172.16.0.0` (Class B, số bit mạng mặc định: $N = 16$).
* **Số bit mượn:** $S = 20 - 16 = 4$ bit $\implies$ Tạo ra $2^4 = 16$ mạng con cỡ `/20`.
* **Subnet Mask (/20):** `255.255.240.0` (Vết cắt tại Octet 3, Bước nhảy $\text{MN} = 2^4 = 16$).

---

## 2. Các Bước Chia VLSM (Theo Thứ Tự Nhu Cầu)

### Phân vùng 1: Kho bãi & Vận hành (480 hosts)
* $2^9 - 2 = 510 \ge 480 \implies H = 9$ bit $\implies P = 32 - 9 = \mathbf{/23}$.
* Bước nhảy tại Octet 3: $2^{24 - 23} = 2^1 = 2$.
* Subnet Mask: `255.255.254.0`
* **Subnet ID:** `172.16.0.0/23`
* **Dải IP sử dụng được:** `172.16.0.1` – `172.16.1.254`
* **Broadcast ID:** `172.16.1.255`

### Phân vùng 2: Khối Văn phòng & CSKH (115 hosts)
* Mốc bắt đầu: `172.16.2.0`
* $2^7 - 2 = 126 \ge 115 \implies H = 7$ bit $\implies P = 32 - 7 = \mathbf{/25}$.
* Bước nhảy tại Octet 4: $2^{32 - 25} = 2^7 = 128$.
* Subnet Mask: `255.255.255.128`
* **Subnet ID:** `172.16.2.0/25`
* **Dải IP sử dụng được:** `172.16.2.1` – `172.16.2.126`
* **Broadcast ID:** `172.16.2.127`

### Phân vùng 3: Máy chủ & Quản trị (28 hosts)
* Mốc bắt đầu: `172.16.2.128`
* $2^5 - 2 = 30 \ge 28 \implies H = 5$ bit $\implies P = 32 - 5 = \mathbf{/27}$.
* Bước nhảy tại Octet 4: $2^{32 - 27} = 2^5 = 32$.
* Subnet Mask: `255.255.255.224`
* **Subnet ID:** `172.16.2.128/27`
* **Dải IP sử dụng được:** `172.16.2.129` – `172.16.2.158`
* **Broadcast ID:** `172.16.2.159`

### Phân vùng 4: Link WAN HQ - Branch (2 hosts)
* Mốc bắt đầu: `172.16.2.160`
* $2^2 - 2 = 2 \ge 2 \implies H = 2$ bit $\implies P = 32 - 2 = \mathbf{/30}$.
* Bước nhảy tại Octet 4: $2^{32 - 30} = 2^2 = 4$.
* Subnet Mask: `255.255.255.252`
* **Subnet ID:** `172.16.2.160/30`
* **Dải IP sử dụng được:** `172.16.2.161` – `172.16.2.162`
* **Broadcast ID:** `172.16.2.163`

---

## 3. Bảng Quy Hoạch Tổng Thể (LLD Table)

| Phân vùng chức năng | Nhu cầu | Prefix | Subnet Mask (DDN) | Nơi cắt & Bước nhảy | Subnet ID | Dải IP khả dụng | Broadcast ID |
| :--- | :---: | :---: | :--- | :--- | :--- | :--- | :--- |
| **Kho bãi & Vận hành** | 480 | `/23` | `255.255.254.0` | Octet 3, MN = 2 | `172.16.0.0` | `172.16.0.1` – `172.16.1.254` | `172.16.1.255` |
| **Khối Văn phòng & CSKH** | 115 | `/25` | `255.255.255.128` | Octet 4, MN = 128 | `172.16.2.0` | `172.16.2.1` – `172.16.2.126` | `172.16.2.127` |
| **Máy chủ & Quản trị** | 28 | `/27` | `255.255.255.224` | Octet 4, MN = 32 | `172.16.2.128` | `172.16.2.129` – `172.16.2.158` | `172.16.2.159` |
| **Link WAN (Point-to-Point)** | 2 | `/30` | `255.255.255.252` | Octet 4, MN = 4 | `172.16.2.160` | `172.16.2.161` – `172.16.2.162` | `172.16.2.163` |
| **Dải IP dự phòng chưa dùng** | - | - | - | - | `172.16.2.164` | `172.16.2.164` – `172.16.15.254` | `172.16.15.255` |

---

## 4. Chính Sách Phân Bổ Nội Bộ (Áp dụng cho Phân vùng 2)
* **Dải Static IP (20 địa chỉ):** `172.16.2.1` – `172.16.2.20` (Gateway, Switch, AP, Printer).
* **Dải DHCP Pool (100 địa chỉ):** `172.16.2.21` – `172.16.2.120` (Cấp phát động cho 50 nhân viên).
* **Dải Reserved (6 địa chỉ):** `172.16.2.121` – `172.16.2.126` (Dự phòng mở rộng).
