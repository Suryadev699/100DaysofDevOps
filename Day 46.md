## Day 46: Deploy an Application on Docker Containers

The Application development team recently finished development of one of the apps that they want to deploy on a containerized platform. The Application development and DevOps teams met to discuss some of the basic pre-requisites and requirements to complete the deployment. The team wants to test the deployment on one of the app servers before going live and set up a complete containerized stack using a docker compose fie. Below are the details of the task:

On App Server in Datacenter create a docker compose file "/opt/devops/docker-compose.yml" (should be named exactly).
The compose should deploy two services (web and DB), and each service should deploy a container as per details below:

For web service:
a. Container name must be "php_blog".
b. Use image "php" with any "apache" tag.
c. Map "php_blog" container's port "80" with host port "6400".
d. Map "php_blog" container's "/var/www/html" volume with host volume "/var/www/html".

For DB service:
a. Container name must be "mysql_blog".
b. Use image "mariadb" with any tag (preferably "latest"). Check here for more details.
c. Map "mysql_blog" container's port "3306" with host port "3306".
d. Map "mysql_blog" container's "/var/lib/mysql" volume with host volume "/var/lib/mysql".
e. Set MYSQL_DATABASE=database_blog and use any custom user ( except root ) with some complex password for DB connections.

After running docker-compose up you can access the app with curl command curl <server-ip or hostname>:6400/

## Solution:

In the application server, create the directory structure for the application and the Docker Compose file:

```bash
nano /opt/devops/docker-compose.yml
```

Add the following content to the `docker-compose.yml` file:

```yaml
version: '3.8'

services:
  web:
    container_name: "php_blog"
    image: "php:apache"
    ports:
      - "6400:80"
    volumes:
      - "/var/www/html:/var/www/html"

  db:
    container_name: "mysql_blog"
    image: "mariadb:latest"
    ports:
      - "3306:3306"
    volumes:
      - "/var/lib/mysql:/var/lib/mysql"
    environment:
      MYSQL_DATABASE: "database_blog"
      MYSQL_USER: "custom_user"
      MYSQL_PASSWORD: "complex_password"
      MYSQL_ROOT_PASSWORD: "root_password"
```

Save and exit the file.

Now, navigate to the directory where the `docker-compose.yml` file is located and run the following command to deploy the services:

```bash
cd /opt/devops
docker-compose up -d
```

This command will start the containers in detached mode. You can access the application by sending a request to `http://<server-ip-or-hostname>:6400/`.

```bash
curl http://localhost:6400/
```

You should see the default PHP page if everything is set up correctly.
