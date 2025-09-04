## Day 12: Linux Network Services

Our monitoring tool has reported an issue in Stratos Datacenter. One of our app servers has an issue, as its Apache service is not reachable on port 8085 (which is the Apache port). The service itself could be down, the firewall could be at fault, or something else could be causing the issue.

Use tools like telnet, netstat, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

Once fixed, you can test the same using command 
```bash
curl http://stapp01:8085
```
from jump host.

Note: Please do not try to alter the existing index.html code, as it will lead to task failure.

1. Check if Apache is running on the server:
    ```bash
    systemctl status httpd
    ```
    If it's not running, start it:
    ```bash
    systemctl start httpd
    ```

2. Verify that Apache is listening on the correct port (8085):
    ```bash
    netstat -tuln | grep 8085
    ```

3. Here the port 8085 is being used by another service. To make Apache use this port, identify the process using it is not critical and try to stop it:
    ```bash
    sudo kill -9 <PID>
    ```
    Replace the "PID" with the actual process ID that is using port 8085.

4. Restart Apache to bind it to port 8085:
    ```bash
    sudo systemctl restart httpd
    ```

Now we are able to access Apache on port 8085 from the jump host.