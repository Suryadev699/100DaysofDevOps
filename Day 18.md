## Day 18: Configure LAMP server

tasks are as follows:
  a. Install Apache, MyMariaDB, and PHP
  b. Configure Apache to serve a simple PHP website
  c. Secure MariaDB installation

1. Install apache, php and its dependencies
```bash
sudo yum install httpd php php-mysqlnd php-fpm -y
```
2. Start and enable apache service
```bash
sudo systemctl start httpd
```
3. Change the listening port to the specified port:
```bash
sudo sed -i 's/Listen 80/Listen 8080/' /etc/httpd/conf/httpd.conf
```
4. Install MariaDB server
```bash
sudo yum install mariadb-server -y
```
5. Start and enable MariaDB service
```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```
6. Create a database and user also grant all privileges to the user
```bash
sudo mysql -e "CREATE DATABASE mydb;"
sudo mysql -e "CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypassword';"
sudo mysql -e "GRANT ALL PRIVILEGES ON mydb.* TO 'myuser'@'localhost';"
sudo mysql -e "FLUSH PRIVILEGES;"
```
7. Secure MariaDB installation
```bash
sudo mysql_secure_installation
```
Follow the prompts to set the root password, remove anonymous users, disallow root login remotely, remove test database, and reload privilege tables.

