## Day 15: Setup SSL for Nginx

The scenario is to set up SSL for an Nginx web server using a self-signed certificate.

There are two files '.crt' and '.key' in the /tmp directory on jump host. The '.crt' file is the self-signed certificate, and the '.key' file is the private key.

1. Install Nginx on the app server specified:
    ```bash
    sudo yum update -y && sudo yum install nginx -y
    ```

2. Edit the nginx configuration file to set up SSL:
    - use the editor of your choice, open '/etc/nginx/nginx.conf'
    - at the server block, set the ip to <app server ip> and uncomment the settings for TLS enabled server block and add the same ip <app server ip>
    - change the name of the certificate and key file.
    - the final server block should look like this:
    ```nginx
    server {
        listen       80;
        listen       443 ssl;
        server_name  <app server ip>;
        ssl_certificate      /tmp/cert.crt;
        ssl_certificate_key  /tmp/cert.key;
        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;
        ssl_ciphers  HIGH:!aNULL:!MD5;

        location / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }
    }
    ```

3. Copy the certificate and key files from the jump host to the app server:
    ```bash
    scp /tmp/cert.crt user@<app server ip>:/tmp/
    scp /tmp/cert.key user@<app server ip>:/tmp/
    ```

4. Start and enable Nginx service:
    ```bash
    sudo systemctl start nginx && sudo systemctl enable nginx
    ```

5. Curl the app server to verify:
    ```bash
    curl -k https://<app server ip>
    ```

You have successfully set up SSL for Nginx using a self-signed certificate.