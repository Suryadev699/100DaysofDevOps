## Day 39: Create a Docker Image From Container

Task is to create an Docker image from a running container.

```bash
sudo docker commit <container_id> my_image:my_tag
```
This command creates a new image named `my_image` with the tag `my_tag` from the specified container ID. You can then run a new container from this image using.