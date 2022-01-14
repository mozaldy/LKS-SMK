# TUTORIAL KISI-KISI LKS Provinsi Jawa Barat
## Modul 1
### 1. Konfigurasi VPC
- Penjelasan  
  ```Membuat VPC dimana kita akan menempatkan semua proyek kita pada modul ini, kita akan membuat; VPC, Subnet Public, Subnet Private, Internet Gateway, NAT, Route Table```
- Langkah-Langkah
  - Konfigurasi VPC
    - Name: `VPC-1`
    - CIDR Block: `10.100.0.0/16`
  - Konfigurasi Subnet
    - Penjelasan  
    ```Membuat 4 Subnet, 2 Public dan 2 Private seperti yang diperintahkan dalam modul```
    - Subnet Public A
      - Nama: `Public Subnet A`
      - Availability Zone: 'A'
      - IPv4 CIDR: `10.100.10.0/24`
    - Subnet Private B
      - Nama: `Private Subnet A`
      - Availability Zone: 'A'
      - IPv4 CIDR: `10.100.11.0/24`
    - Subnet Public A
      - Nama: `Public Subnet B`
      - Availability Zone: 'B'
      - IPv4 CIDR: `10.100.20.0/24`
    - Subnet Private B
      - Nama: `Private Subnet B`
      - Availability Zone: 'B'
      - IPv4 CIDR: `10.100.21.0/24`
   - Konfigurasi Internet Gateway
     - Penjelasan  
     ```Internet Gateway dibutuhkan untuk menghubungkan Public Subnet ke intenet```
     - Nama: `IGW-VPC-1`
     - Attach to VPC-1
   - Konfigurasi NAT Gateway
     - Penjelasan  
     ```NAT Dibutuhkan untuk menghubungkan Subnet Private ke internet menggunakan Subnet Public yang terhubung ke internet sebagai Gatewaynya```
     - NAT-A
       - Nama: `NAT-A`
       - Subnet: `Public Subnet A`
       - Elastic IP Allocation: Pilih salah satu, klik `Allocate Elastic IP` bila tidak tersedia
     - NAT-B
       - Nama: `NAT-B`
       - Subnet: `Public Subnet B`
       - Elastic IP Allocation: Pilih salah satu, klik `Allocate Elastic IP` bila tidak tersedia
  - Konfigurasi Route Table (Menyesuaikan dengan berapa subnet yang sudah dibuat)
    - RT Public A
      - Nama: `RT Public A`
      - Availability Zone: 'A'
      - IPv4 CIDR: `10.100.10.0/24`
      - Add Route:
        - Destination: `0.0.0.0/0` (artinya internet)
        - Target: Internet Gateway -> `IGW-VPC-1`
    - RT Private A
      - Nama: `RT Private A`
      - Availability Zone: 'A'
      - IPv4 CIDR: `10.100.11.0/24`
      - Add Route:
        - Destination: `0.0.0.0/0` (artinya internet)
        - Target: NAT Gateway -> `NAT-A`
    - RT Public B
      - Nama: `RT Public B`
      - Availability Zone: 'B'
      - IPv4 CIDR: `10.100.20.0/24`
      - Add Route:
        - Destination: `0.0.0.0/0` (artinya internet)
        - Target: Internet Gateway -> `IGW-VPC-1`
    - RT Private B
      - Nama: `RT Private B`
      - Availability Zone: 'B'
      - IPv4 CIDR: `10.100.21.0/24`
      - Add Route:
        - Destination: `0.0.0.0/0` (artinya internet)
        - Target: NAT Gateway -> `NAT-A`
   
