## Day 42: Create a Docker Network

Today, we will create a custom Docker network and connect multiple containers to it.

```bash
# To create a newtork enter the below command
sudo docker network create -d bridge <network_name> --subnet <subnet> --gateway <gateway_ip>
```
Replace `<network_name>`, `<subnet>`, and `<gateway_ip>` with your desired values.