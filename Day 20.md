## Day 20: Configure Nginx + PHP-FPM Using Unix Sock

The  application development team is planning to launch a new PHP-based application, which they want to deploy on  infra in DC. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure. Below are the requirements they shared:

a. Install nginx on app server , configure it to use port 8094 and its document root should be /var/www/html.

b. Install php-fpm version 8.1 on app server, it must use the unix socket /var/run/php-fpm/default.sock (create the parent directories if don't exist).

c. Configure php-fpm and nginx to work together.

d. Once configured correctly, you can test the website using curl http://appserver:8094/index.php command from jump host.

1. Install nginx and PHP-FPM if they are not already installed:
   ```bash
   sudo apt update
   sudo apt install nginx php:8.1/common
   ```
2. Configure PHP-FPM to use a Unix socket:
    - Edit /etc/php-fpm.d/www.conf
    - Edit the user and group to 'nginx'
    - Change the listen directive to the specified port
        ```ini
        user = nginx
        group = nginx
        listen = /var/run/php-fpm/php-fpm.sock
        listen.owner = nginx
        listen.group = nginx
        listen.mode = 0660
        ```
    - Save and close the file.
    - Set the directory permission on php files to nginx group
        ```bash
        sudo chown -R :nginx /path/to/your/php/files
        sudo chmod -R 750 /path/to/your/php/files
        ```
3. Configure Nginx to use the Unix socket:
    - Edit the port number, HTML Directory, location block for php files, and the index directive in the server block of /etc/nginx/nginx.conf
        ```nginx
        server {
            listen       80;
            server_name  your_domain.com;
            root         /var/www/html;

            index index.php index.html index.htm;

            location / {
                try_files $uri $uri/ =404;
            }

            location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
            }
        }
        ```
    - Save and close the file.
4. Configure php-fpm nginx module
    - Open /etc/nginx/conf.d/php-fpm.conf
    - Add the port specified.
    - Restart the services to apply the changes:
        ```bash
        sudo systemctl restart php-fpm
        sudo systemctl restart nginx
        ```


