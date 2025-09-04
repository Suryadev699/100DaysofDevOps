## Day 14: Linux Process Troubleshooting

This scenario is to check the linux process and troubleshoot 
accordingly. There is a service that is not working as expected in one of the app servers. Inspect the server and fix the issue.

Here in this scenario, there is apache service which is not running. Let's troubleshoot and fix it.

1. First check the status of the service:
    - Log into the specified app server.
    - Run the command to check the status of the apache service:
    ```bash
    sudo systemctl status apache2
    ```
    - This displays the current status of the service.

Now from the output, we can see that the service is unable to start on the specified port. 

2. Check if the port is already in use:
    - Run the command;
    ```bash
    sudo netstat -tulnp | grep <port specified>
    ```
    - This will show the process using the port.
    - Identify the process is critical or not and take necessary action.

3. Kill the process using the port:
    - If the process is not critical, you can kill it using:
    ```bash
    sudo kill -9 <PID>
    ```
    - Replace `<PID>` with the actual Process ID obtained from the previous command.

4. Restart the apache service. 
    ```bash
    sudo systemctl start apache2
    ```

Now the service is running and able to accept connections on the specified port.

