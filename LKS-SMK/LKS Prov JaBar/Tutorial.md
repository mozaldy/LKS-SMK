# TUTORIAL KISI-KISI LKS Provinsi Jawa Barat
## Modul 1
### 1. Konfigurasi VPC
- #### Penjelasan  
  ```Membuat VPC dimana kita akan menempatkan semua proyek kita pada modul ini, kita akan membuat; VPC, Subnet Public, Subnet Private, Internet Gateway, NAT, Route Table```
- #### Langkah-Langkah
  - Konfigurasi VPC
    - Name: `VPC-1`
    - CIDR Block: `10.100.0.0/16`
    - Edit DNS Hostnames menjadi 'Enable' menggunakan tombol `Actions`
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
     - Nama: `VPC-1-IGW`
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
      - Subnet Assosiation: Subnet Public A
      - Add Route:
        - Destination: `0.0.0.0/0` (artinya internet)
        - Target: Internet Gateway -> `IGW-VPC-1`
    - RT Private A
      - Nama: `RT Private A`
      - Availability Zone: 'A'
      - IPv4 CIDR: `10.100.11.0/24`
      - Subnet Assosiation: Subnet Private A
      - Add Route:
        - Destination: `0.0.0.0/0` (artinya internet)
        - Target: NAT Gateway -> `NAT-A`
    - RT Public B
      - Nama: `RT Public B`
      - Availability Zone: 'B'
      - IPv4 CIDR: `10.100.20.0/24`
      - Subnet Assosiation: Subnet Public B
      - Add Route:
        - Destination: `0.0.0.0/0` (artinya internet)
        - Target: Internet Gateway -> `IGW-VPC-1`
    - RT Private B
      - Nama: `RT Private B`
      - Availability Zone: 'B'
      - IPv4 CIDR: `10.100.21.0/24`
      - Subnet Assosiation: Subnet Private B
      - Add Route:
        - Destination: `0.0.0.0/0` (artinya internet)
        - Target: NAT Gateway -> `NAT-A`
### 2. Konfigurasi EC2
- #### Penjelasan  
  ```Disini kita akan mengkonfigurasi Instances, Load Balancing, dan Auto Scaling yang harus lakukan untuk menyelesaikan modul ini```
- #### Langkah-langkah
  - Konfigurasi Target Group
    - Target Type: `Instances`
    - Name: `myTargetGroup`
    - VPC: VPC-1
    - Create Target Group
  - Konfigurasi Load Balander
    - Select `Application Load Balancer`
    - Name: `myLoadBalancer`
    - VPC: Pilih 2 Subnet yang Public
    - Security Group: Buat Baru dengan nama `'Security Group A'`
      - Penjelasan
      ```Membuat security group untuk load balancer sesuai perintah di modul```
      - Add Rule
        - Type: `HTTP`
        - Source: `Anywhere`
  - Konfigurasi Launch Template
    - Name: UbuntuServerWeb
    - Application and OS Images 
      - `Quick Start -> Ubuntu`
      - AMI -> Pilih Terbaru
    - Instance Type
      - Pilih `Free tier eligible` dengan Spek tertinggi
    - Network Setting
      - `Don't include in launch template`
    - Security Group 
      - Buat baru dengan nama 'Security Group B'
      - Add rule: HTTP dari Anywhere dan SSH dari Anywhere (anywhere artinya internet)
    - Advanced Details -> User data
      #### Copy text dibawah
      ```
      #!/bin/bash
      sudo apt-get update -y
      sudo apt-get install apache2 -y
      sudo apt install php libapache2-mod-php -y
      sudo systemctl restart apache2
      sudo chmod -R 777 /var/www/html
      sudo mv /var/www/html/index.html /var/www/html/index.php
      sudo echo "<?php phpinfo(); ?>" > /var/www/html/index.php
      sudo systemctl start apache2
      ```
  - Konfigurasi Target Group
    - Step 1
      - Name: `myAutoScaling`
      - Launch Template: `UbuntuServerWeb`
      - Next
    - Step 2
      - VPC: `VPC-1`
      - Availability Zones and Subnets: `Private Subnet A` Dan `Private Subnet B`
      - Next
    - Step 3
      - Load Balancing: `Attach to an existing load balancer` -> `Choose from your load balancer target groups` -> `myTargetGroup | HTTP`
      - Next
    - Step 4
      - Group Size:
       -  Desired Capacity: `2`
       -  Minimum Capacity: `1`
       -  Maximum Capacity: `6`
      - Scaling policies: `Target tracking scaling policy`
      - Next
    - Step 5 - terakhir
      - Next
### Modul 2
#### Konfigurasi VPC
   - Konfigurasi VPC
   - Konfigurasi Subnet
   - Konfigurasi Internet Gateway
   - Konfigurasi NAT
   - Konfigurasi Route Table
#### Konfigurasi EC2
```
#!/bin/bash
sudo apt-get update -y
sudo apt-get install apache2 -y
sudo apt install php libapache2-mod-php -y
sudo systemctl restart apache2
sudo chmod -R 777 /var/www/html
sudo mv /var/www/html/index.html /var/www/html/index.php
sudo echo "<?php phpinfo(); ?>" > /var/www/html/index.php
sudo systemctl start apache2
```
#### Konfigurasi Mount EFS
```    
sudo apt-get -y install git binutils
git clone https://github.com/aws/efs-utils
cd /path/efs-utils
./build-deb.sh
sudo apt-get -y install ./build/amazon-efs-utils*deb
```      
Mount dengan NFS Client
```      
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 
fs-057b31a12ae56e1cb.efs.ap-southeast-1.amazonaws.com:/ efs
```
