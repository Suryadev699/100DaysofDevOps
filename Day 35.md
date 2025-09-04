## Day 35: Install Docker Packages and Start Docker Service

The task is to install the docker and docker-compose packages, and then start the Docker service.

Before installing the docker packages, make sure which flavour of linux you are using. The commands to install packages differ between Debian-based and Red Hat-based distributions.

```bash
# For Debian-based distributions
sudo apt update
sudo apt install docker.io docker-compose
sudo systemctl start docker
sudo systemctl enable docker

# For Red Hat-based distributions
sudo yum install docker docker-compose
sudo systemctl start docker
sudo systemctl enable docker
```

After running the above commands, Docker and Docker compose should be installed and running. 

