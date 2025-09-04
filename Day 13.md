## Day 13: IPtables installation and configuration

We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 8082 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:

    a. Install iptables and all its dependencies on each app host.
    b. Block incoming port 8082 on all apps for everyone except for LBR host.
    c. Make sure the rules remain, even after system reboot.


1. **Install IPtables**:
    ```bash
    sudo yum update -y && sudo yum install iptables-services -y
    ```

2. **Start and enable IPtables service**:
    ```bash
    sudo systemctl start iptables
    sudo systemctl enable iptables
    ```

3. **Add rule to allow traffic from the LBR host:
    ```bash
    sudo iptables -A INPUT -p tcp -s <LBR IP> --dport 8082 -j ACCEPT
    sudo iptables -A INPUT -p tcp -m tcp --dport 8082 -j DROP
    ```
4. **Save and verif the rules**:
    ```bash
    sudo service iptables save
    sudo iptables -L -n --line-numbers
    ```

Restart the service to apply the changes.