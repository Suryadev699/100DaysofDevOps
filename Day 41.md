## Day 41: Write a Docker File

The main objective of this task is to write a Docker file that takes ubuntu as image, install apache2 webserver change the listening port and run the webserver.

```bash
vi Dockerfile
```

```dockerfile
# Use Ubuntu as the base image
FROM ubuntu

# Update the repository sources list
RUN apt-get update -y

# Install Apache2
RUN apt-get install -y apache2

# Listen on port 8087
RUN echo "Listen 8087" >> /etc/apache2/ports.conf

# Start Apache service
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

This completes the Dockerfile creation. 

Additionally, you can build and run the Docker image using the following commands:

```bash
# Build the Docker image
docker build -t my-apache2-image .
# Run the Docker container
docker run -d -p 8087:8087 my-apache2-image
```

This Dockerfile uses the official Ubuntu image, updates the package list, installs Apache2, modifies the listening port to 8087, and starts the Apache service in the foreground.