## Day 47: Docker Python App

A python app needed to be Dockerized, and then it needs to be deployed on App Server. We have already copied a requirements.txt file (having the app dependencies) under /python_app/src/ directory on App Server. Further complete this task as per details mentioned below:

Create a Dockerfile under /python_app directory:

Use any python image as the base image.
Install the dependencies using requirements.txt file.
Expose the port 6300.
Run the server.py script using CMD.

Build an image named nautilus/python-app using this Dockerfile.
Once image is built, create a container named pythonapp_nautilus:
Map port 6300 of the container to the host port 8098.

Once deployed, you can test the app using curl command on App Server. "curl http://localhost:8098/"

## Solution:

Login to the App server and navigate to '/python_app' directory:

create the Dockerfile:

```Dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY src/ .

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 6300

CMD ["python", "server.py"]
```

Build the Docker image named 'nautilus/python-app':

```bash
docker build -t nautilus/python-app .
```

Once the image is build, create and run the container named as specified:

```bash
docker run -d -p 8098:6300 --name pythonapp_nautilus nautilus/python-app
```

The container should be running now. You can verify it using:

```bash
docker ps # to see the status of running container

curl http://localhost:8098/ # to test the app

```

You now have a python app running inside a Docker container, accessible via port 8098 on the host machine.
