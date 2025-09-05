## Day 44: Write a Docker Compose File.

The application development team shares a static web content that needs to host on a httpd websetver using a containerised platform. The team has shared details to Devops team and we need to set an environment accordingl to those guidelines. Below are the details:

a. On an app server, create a container named httpd using a docker compose file "/opt/docker-compose.yml".

b. Use httpd(latest) image for container and make sure it is named and httpd; you can use any name for service.

c. Map port 80 of container with port 6200 of docker host.

d. Map containers /usr/local/apacje2/htdocs volume with /opt/demo volume of the docker host which is already present.


## Solution:

1. Create a docker compose file with the name docker-compose.yml under /opt directory.

```bash
sudo vi /opt/docker-compose.yml
```
This opens up an editor, add the following lines to it.

```yaml
version: '3'
services:
    httpd:
        image: httpd:latest
        container_name: httpd
        ports:
            - "6200:80"
        volumes:
            - /opt/demo:/usr/local/apache2/htdocs
```
2. Save and exit the file.

3. Run the docker compose file using the below command.

```bash
sudo docker-compose -f /opt/docker-compose.yml up -d
```
4. Verify the container is up and running.

```bash
sudo docker ps
```

You now have a running httpd container with the specified configurations. You can access the web server using the host's IP i.e., 
```bash
curl http://localhost:6200
```
