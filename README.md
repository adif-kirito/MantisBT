# MantisBT

Mantis Bug Tracker installation on Ubuntu
<br>

<!-- ----------------------------------------------------- -->
## Step 1: Install Apache2, PHP and Database Server

### Install PHP dependencies
```
sudo apt update
```
<br>

```
sudo apt install vim wget php php-cli php-fpm php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath
```
<br>

### Install Apache2 Web Server
```
sudo apt -y install apache2
```
<br>

### Install MariaDB database server
```
sudo apt install mariadb-server mariadb-client
```
<br>

### Update authentication plugin for root user
```
sudo mysql -u root
```
<br>

```mysql
UPDATE mysql.user SET plugin = 'mysql_native_password' WHERE User = 'root';
```
```mysql
FLUSH PRIVILEGES;
```
```mysql
QUIT;
```
<br>

### Secure database server
```
sudo mysql_secure_installation
```
Change root password, and only select Y
<br>

### Login to the MariaDB shell
```
mysql -u root -p
```
<br>

###Create a database and user for MantisBT
```mysql
CREATE USER 'mantisbt'@'localhost' IDENTIFIED BY 'changePassword';
```
```mysql
CREATE DATABASE mantisbt;
```
```mysql
GRANT ALL PRIVILEGES ON mantisbt.* TO 'mantisbt'@'localhost';
```
```mysql
FLUSH PRIVILEGES;
```
```mysql
QUIT
```
<br>

### Check if you can log in to Database shell as mantisbt user
```
CREATE USER 'mantisbt'@'localhost' IDENTIFIED BY 'changePassword';
```
<br>

<!-- ----------------------------------------------------- -->
## Step 2: Download and Install Mantis Mantis Bug Tracker

### Download package to your Ubuntu
```
wget https://sourceforge.net/projects/mantisbt/files/latest/download
```
<br>

### Unzip the package
```
unzip download
```
<br>

### Move the folder to /var/www/mantis directory
```
mv mantisbt-2.21.1 /var/www/mantis
```
<br>

### Set proper permissions for the directory
```
chown -R www-data.www-data /var/www/mantis
```
```
chmod -R 755 /var/www/mantis
```
<br>

### Create Apache Virtual Host file for Mantis Bug Tracker
```
sudo vim /etc/apache2/sites-available/mantis.conf
```
```
<VirtualHost *:80>
        DocumentRoot "/var/www/mantis"
        ServerName IP
        <Directory /var/www/mantis>
                allowOverride all
                allow from all
        </Directory>
</VirtualHost>
```
```
a2dissite 000-default.conf
```
```
a2ensite mantis.conf
```
```
a2enmod rewrite
```
<br>

Restart apache2 service
```
systemctl restart apache2
```
<br>

<!-- ----------------------------------------------------- -->
## Step 3: Configure smtp in mantis config file

```
cd /var/www/mantis/config
```
```
sudo vi config_inc.php
```
<br>

```
$g_allow_signup  = ON;
$g_enable_email_notification = ON;
$g_phpMailer_method = PHPMAILER_METHOD_SMTP;
$g_smtp_host = 'smtp.gmail.com';
$g_smtp_connection_mode = 'tls';
$g_smtp_port = 587;
$g_smtp_username = 'test@gmail.com'; //your email address
$g_smtp_password = 'abc'; //your app generate password, refer method 1(https://www.cubebackup.com/blog/how-to-use-google-smtp-service-to-send-emails-for-free/)
$g_administrator_email = 'test@gmail.com'; //your email address
$g_log_level = LOG_EMAIL | LOG_EMAIL_RECIPIENT | LOG_FILTERING | LOG_AJAX;
$g_log_destination = "file:/var/www/mantis/mantisbt.log";
```
<br>

<!-- ----------------------------------------------------- -->
# Screenshot

![Alt text](https://github.com/adif-kirito/MantisBT/blob/main/pic/login.PNG)
<br>
<br>
![Alt text](https://github.com/adif-kirito/MantisBT/blob/main/pic/index.PNG)
