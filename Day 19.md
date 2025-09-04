## Day19: Install and Configure web application

Corp Industries is planning to host two static websites on their infra in Datacenter. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:
a. Install httpd package and dependencies on application server.
b. Apache should serve on port 3003.
c. There are two website's backups /home/user/media and /home/user/demo on jump_host. Set them up on Apache in a way that media should work on the link http://localhost:3003/media/ and demo should work on link http://localhost:3003/demo/ on the mentioned app server.
d. Once configured you should be able to access the website using curl command on the respective app server, i.e curl http://localhost:3003/media/ and curl http://localhost:3003/demo/

1. Install packages on application server:
```bash
sudo yum install httpd wget -y
```
2. Configure Apache to listen on port 3003:
```bash
sudo sed -i 's/Listen 80/Listen 3003/' /etc/httpd/conf/httpd.conf
```
3. Create directories for the websites:
```bash
sudo mkdir -p /var/www/html/media
sudo mkdir -p /var/www/html/demo
```
4. Copy website backups from jump_host to application server:
```bash
scp user@jump_host:/home/user/media/* /var/www/html/media/
scp user@jump_host:/home/user/demo/* /var/www/html/demo/
```
5. Set proper permissions:
```bash
sudo chown -R apache:apache /var/www/html/media
sudo chown -R apache:apache /var/www/html/demo
```
6. Start and enable Apache service:
```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```
7. Verify the setup using curl:
```bash
curl http://localhost:3003/media/
curl http://localhost:3003/demo/
```
8. If everything is set up correctly, you should see the content of the media and demo websites in the curl output.


