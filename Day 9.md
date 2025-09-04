## Day 9: MariaDB Troubleshooting

The scenario involves a MariaDB server that is experiencing performance issues. The goal is to identify the root cause of the problem and implement a solution to improve the server's performance.

1. **Identify the Problem**:
    ```bash
    sudo systemctl status mariadb
    ```
    This will show if the MariaDB service is running and any recent errors.

2. **Check Logs**:
    The logs can provide insights into what might be going on with this service.

    Logs are stored at `/var/log/mysql/error.log` or `/var/log/mariadb/mariadb.log`.

    ```bash
    sudo tail -n 50 /var/log/mysql/error.log
    ```

3. **Analyze the Logs**:
    Here in this scenario, the logs indicate that the service does not have enough permissions to run correctly.

    ```bash
    sudo chown -R mysql:mysql /var/lib/mysql
    ```

4. **Restart the Service**:
    After fixing the permissions, restart the MariaDB service.

    ```bash
    sudo systemctl restart mariadb
    ```

Now checking the status again should show that the service is running without issues.