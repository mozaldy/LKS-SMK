<p align="center">
    <img width=250 weigth=250 src="https://github.com/aquabellus/persiapan/blob/master/images/Logo-Color.png" /><br>
    <div align="center">
        <i>Just Ordinary People</i><br>
        <i>SMKN 1 Banyuwangi</i>
    </div>
</p>

# Daftar Isi
- [Daftar Isi](#daftar-isi)
- [Hari Kedua](#hari-kedua)
  - [Elastic Cloud Computing](#elastic-cloud-computing)
    - [Konfigurasi VPC (SKIP BILA MENGGUNAKAN VPC Default)](#konfigurasi-vpc)
      - [Membuat VPC](#membuat-vpc)

---
# Hari Kedua
## Elastic Cloud Computing
### Konfigurasi VPC
#### Membuat VPC
- Langkah-Langkah
  - Masuk ke service VPC -> Klik Menu "Your VPCs" di sidebar kiri -> Klik tombol "Create VPC"
    - Name tag: `NAMA YANG INGIN DIGUNAKAN, CONTOH: 'VPC-A'`
    - IPv4 CIDR: `CIDR YANG INGIN DIGUNAKAN (HARUS SAMA DENGAN SUBNET), CONTOH: '10.100.0.0/16'`
#### Membuat Subnet
- Langkah-Langkah
  - Klik menu "Subnet" di sidebar kiri -> Klik tombol "Create Subnet"
    - VPC ID: `PILIH VPC YANG INGIN DI KAITKAN KE SUBNET`
    - Subnet Name: `NAMA YANG INGIN DIGUNAKAN, CONTOH: 'VPC-A-Subnet'`
    - IPv4 CIDR Block: `SUBNET IPv4 YANG INGIN DIGUNAKAN (HARUS DALAM JANGKAUAN CIDR VPC), CONTOH: '10.100.0.0/24'`
#### Membuat Internet Gateway
- Langkah-Langkah
  - Klik menu "Internet Gateway" -> Klik tombol "Create Internet Gateway"
    - Name tag: `NAMA YANG INGIN DIGUNAKAN, Contoh: 'VPC-A-IGW'`
    - Kembali ke menu "Internet Gateway" -> Klik Internet Gateway yang sudah dibuat -> Klik Actions -> Attach to VPC -> Pilih VPC Yang dibuat -> Attach Internet Gateway
#### Membuat Route Tables
- Langkah-Langkah
  - Klik menu "Route Tables" -> Klik tombol "Create Route Table"
    - Name tag: `NAMA YANG INGIN DIGUNAKAN, Contoh: 'VPC-A-RT'`
    - VPC: `PILIH VPC YANG INGIN DIGUNAKAN`
  - Klik "Edit Routes" -> Add Route -> `Destinasi = Internet (0.0.0.0/0)` - `Target = Internet Gateway -> (VPC-A-IGW)`
    
