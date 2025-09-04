## Day 43: Docker Ports Mapping

Today we have to map the container's port 80 to the host's port 8080.

```bash
sudo docker run -d --name <container_name> -p <host_port>:<container_port> <image_name>
```

Replace `<container_name>` with your desired container name, `<host_port>` with `8080`, `<container_port>` with `80`, and `<image_name>` with the name of the Docker image you want to run.

