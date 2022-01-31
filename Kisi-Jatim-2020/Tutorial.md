# TUTORIAL KISI-KISI LKS Provinsi Jawa Barat
## EC2
---  
```sh
$ sudo yum update -y  
$ sudo amazon-linux-extras enable php7.4 -y  
$ sudo yum clean metadata  
$ sudo yum install  -y php php-{pear,cgi,common,curl,mbstring,gd,mysqlnd,gettext,bcmath,json,xml,fpm,intl,zip,imap}        
$ php --version  
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
**Install PHPMyAdmin**
```sh
$ wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz
$ mkdir phpMyAdmin && tar -xvzf phpMyAdmin-latest-all-languages.tar.gz -C phpMyAdmin --strip-components 1
$ rm phpMyAdmin-latest-all-languages.tar.gz
$ chmod -R 777 writable uploads
```
**.env configurations.**  
```sh
$ nano .env
#--------------------------------------------------------------------
# APP
#--------------------------------------------------------------------
app.baseURL = 'http://13.250.55.208/'
#--------------------------------------------------------------------
# DATABASE
#--------------------------------------------------------------------
database.default.hostname = rds.ipaddress.host
database.default.database = ci4
database.default.username = root
database.default.password = root
database.default.DBDriver = MySQL
```

**Set the host:**  
```sh
$ sudo vim /etc/httpd/conf/httpd.conf   
```

**Add code at the bottom of the file**  

```blade
<VirtualHost *:80>  
	ServerName codeigniter.example.com  
	DocumentRoot /var/www/html/speedrun/public  
	<Directory /var/www/html/speedrun>  
		AllowOverride All  
	</Directory>  
</VirtualHost>  
```  

```sh
$ sudo systemctl start httpd  
$ sudo systemctl start php-fpm  
$ sudo systemctl enable php-fpm  
```
