## Day 40: Docker EXEC Operations

Todays task is to perform 'docker exec' operation on a running container. install apache2 webserver and change the ports as specified.

```bash
sudo docker ps # to get the container id
```
Copy the container ID from the above output

```bash
sudo docker exec -it <container_id> bash
```
This command will open a bash shell inside the running container.

Now, inside the container, install apache2 webserver:

```bash
apt-get update
apt-get install apache2 -y
```
Next, change the default port of apache2 from 80 to 8080. You can do this by editing the ports.conf file:

```bash
nano /etc/apache2/ports.conf
```
Change the line that says `Listen 80` to `Listen 8080`. Save and exit the file.

Now, restart the apache2 service to apply the changes:

```bash
sudo service apache2 restart
```

Finally, exit the container:

```bash
exit
```
You have successfully performed 'docker exec' operations to install and configure apache2 webserver inside a running container.