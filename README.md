# MantisBT

Mantis Bug Tracker installation on Ubuntu
<br>

<!-- ----------------------------------------------------- -->
## Step 1: Install Apache2, PHP and Database Server

###Install PHP dependencies
```
sudo apt update
```
<br>

```
sudo apt install vim wget php php-cli php-fpm php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath
```
<br>

###Install Apache2 Web Server
```
sudo apt -y install apache2
```
<br>

###Install MariaDB database server
```
sudo apt install mariadb-server mariadb-client
```
<br>

Update authentication plugin for root user
```
sudo mysql -u root
```
<br>

```mmysql
UPDATE mysql.user SET plugin = 'mysql_native_password' WHERE User = 'root';
```
```mmysql
FLUSH PRIVILEGES;
```
```mmysql
QUIT;
```
<br>

Secure database server
```
sudo mysql_secure_installation
```
Change root password, and only select Y
<br>

Login to the MariaDB shell
```
mysql -u root -p
```
<br>

Create a database and user for MantisBT
```mmysql
CREATE USER 'mantisbt'@'localhost' IDENTIFIED BY '**_changePassword_**';
```





<!-- ----------------------------------------------------- -->
## SSH

```
hydra -l <username> -P <full path to pass> MACHINE_IP -t 4 ssh
```
<br>

| Option |               Description              |
|:------:|:--------------------------------------:|
|  `-l`  | specifies the (SSH) username for login |
|  `-P`  |      indicates a list of passwords     |
|  `-t`  |   sets the number of threads to spawn  |

<br>

Example: hydra -l root -P passwords.txt MACHINE_IP -t 4 ssh

<br><br>

<!-- ----------------------------------------------------- -->
## Post Web Form

```
hydra <username> <wordlist> MACHINE_IP http-post-form "<path>:<login_credentials>:<invalid_response>"
```
<br>

|        Option        |                                        Description                                       |
|:--------------------:|:----------------------------------------------------------------------------------------:|
|         `-l`         |                             the username for (web form) login                            |
|         `-P`         |                                 the password list to use                                 |
|   `http-post-form`   |                               the type of the form is POST                               |
|       `<path>`       |                       the login page URL, for example, `login.php`                       |
|  `login_credentials>` | the username and password used to log in, for example, `username=^USER^&password=^PASS^` |
| `<invalid_response>` |                         part of the response when the login fails                        |
|         `-v`         |                             verbose output for every attempt                             |

<br>

Example: hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
<br><br>

## Acknowledgments

* TryHackMe - Hydra (https://tryhackme.com/room/hydra)
