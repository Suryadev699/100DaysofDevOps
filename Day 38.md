## Day 38: Pull Docker Image

Today's task is to pull a Docker image from the Docker hub and run a container using that image. Also to retag the image with a new name.

1. **Pulling the Docker Image**:
   Open your terminal and run the following command to pull the latest Ubuntu image from Docker Hub:
   ```bash
   docker pull ubuntu:latest
   ```
2. **Tagging the Image**:
   After pulling the image, you can tag it with a new name. For example, to tag it as `my-ubuntu:latest`, run:
   ```bash
   docker tag ubuntu:latest my-ubuntu:latest
   ```
3. **Running a Container**:
   Now, you can run a container using the newly tagged image:
   ```bash
   docker run -it my-ubuntu:latest /bin/bash
   ```


