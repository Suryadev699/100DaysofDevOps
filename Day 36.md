## Day 36: Deploy Nginx Container on Application Server

The task is to deploy an Nginx container on the application server with a name specified to it.

To achieve this, follow these steps:
```bash
sudo docker run -d --name <as_specified> <image_name>
```
Example:
```bash
sudo docker run -d --name my_nginx_container nginx
```
This command will run an Nginx container in detached mode with the specified name. Make sure to replace `<as_specified>` with the desired container name and `<image_name>` with the appropriate image name, such as `nginx` for the official Nginx image.

To see the running containers, you can use:
```bash
sudo docker ps
```