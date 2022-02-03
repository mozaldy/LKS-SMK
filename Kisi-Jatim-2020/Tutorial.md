# TUTORIAL KISI-KISI LKS Provinsi Jawa Barat
## EC2
---  
```sh
$ sudo yum update -y  
$ sudo amazon-linux-extras enable php7.4 -y  
$ sudo yum clean metadata  
$ sudo yum install -y php-{pear,cgi,common,curl,mbstring,gd,mysqlnd,gettext,bcmath,json,xml,fpm,intl,zip,imap}        
$ sudo yum install -y httpd
$ sudo yum install -y mariadb
```
**Dependencies installation will take some time. After than set proper permissions on files.**  
```sh
$ sudo chown -R ec2-user:apache /var/www  
$ sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;  
$ find /var/www -type f -exec sudo chmod 0664 {} \;  
$ cd /var/www/html   
```
**Download Web App anda menggunakan s3**
```sh
$ aws s3 cp s3://bucketanda/webanda.zip web
$ unzip web  
$ rm web
$ mv web ../
$ cd ../
$ rmdir html
$ mv web html
$ cd html
```
**Download Web App anda menggunakan Git**
```sh
$ cd /var/www
$ rmdir html
$ sudo yum install -y git
$ git clone https://github.com/potatoedz/WebApp /var/www/html
```
**Install PHPMyAdmin**
```sh
$ wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz
$ mkdir phpMyAdmin && tar -xvzf phpMyAdmin-latest-all-languages.tar.gz -C phpMyAdmin --strip-components 1
$ rm phpMyAdmin-latest-all-languages.tar.gz
$ chmod -R 777 writable uploads
$ sudo systemctl start mariadb
$ sudo mysql_secure_installation
$ sudo systemctl start httpd
```
**.env configurations.**  
```sh
$ nano .env
#--------------------------------------------------------------------
# APP
#--------------------------------------------------------------------
app.baseURL = 'http://13.250.55.208/' (isi ip web server anda)
#--------------------------------------------------------------------
# DATABASE
#--------------------------------------------------------------------
database.default.hostname = localhost (atau) rds.ipaddress.host (bila menggunakan rds)
database.default.database = database anda
database.default.username = root (sesuai saat menjalankan mysql_secure_installation)
database.default.password = root (sesuai saat menjalankan mysql_secure_installation)
database.default.DBDriver = MySQLi (mariadb)
```

**Set host:**  
```sh
$ sudo nano /etc/httpd/conf/httpd.conf   
```

**scroll ke bawah dan ganti **  

```blade
<Directory "/var/www/html">
    # The Options directive is both complicated and important.  Please see
    # http://httpd.apache.org/docs/2.4/mod/core.html#options
    # for more information.
    #
    Options Indexes FollowSymLinks

    #
    # AllowOverride controls what directives may be placed in .htaccess files.
    # It can be "All", "None", or any combination of the keywords:
    #   Options FileInfo AuthConfig Limit
    #
    AllowOverride All

    #
    # Controls who can get stuff from this server.
    #
    Require all granted
</Directory>

```  

```sh
$ sudo systemctl restart httpd    
```
