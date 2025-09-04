## Day 16: Configure Load Balancer (LBR) server for StaticApp

Day by day traffic is increasing on one of the websites managed by the Nautilus production support team. Therefore, the team has observed a degradation in website performance. Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on Nautilus infra in Stratos DC. They started the migration last month and it is almost done, as only the LBR server configuration is pending. Configure LBR server as per the information given below:
a. Install nginx on LBR (load balancer) server.
b. Configure load-balancing with the an http context making use of all App Servers. Ensure that you update only the main Nginx configuration file located at /etc/nginx/nginx.conf.
c. Make sure you do not update the apache port that is already defined in the apache configuration on all app servers, also make sure apache service is up and running on all app servers.
d. Once done, you can access the website using StaticApp button on the top bar.

1. Install nginx on LBR server.
```bash
sudo yum update -y && sudo yum install nginx -y
```
2. Start and enable nginx service.
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```
3. Open the nginx configuration file located at /etc/nginx/nginx.conf using a text editor.
```bash
sudo vi /etc/nginx/nginx.conf
```
4. Add the following configuration to set up load balancing with the app servers. Replace `app_server_1`, `app_server_2`, and `app_server_3` with the actual IP addresses or hostnames of your app servers.
```nginx
http {
    upstream app_servers {
        server app_server_1:80;
        server app_server_2:80;
        server app_server_3:80;
    }
    server {
        listen 80;
        location / {
            proxy_pass http://app_servers;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```
5. Save and close the file.
6. Test the nginx configuration for syntax errors.
```bash
sudo nginx -t
```
7. If the test is successful, reload nginx to apply the changes.
```bash
sudo systemctl reload nginx
```
8. Ensure that the Apache service is running on all app servers.
```bash
sudo systemctl status httpd
```
9. Access the website using the StaticApp button on the top bar to verify that the load balancing is working correctly.

You now have successfully configured the LBR server for the StaticApp with load balancing across multiple app servers.