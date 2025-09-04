## Day 4: Script Execution Permissions

This scenario focuses on managing script execution permissions on a Linux system. Properly setting execution permissions is crucial for security and functionality, ensuring that only authorized users can run specific scripts.

The task is to provide execution permissions to a script named `deploy.sh` located in the `/usr/local/bin/` directory.

```bash
sudo chmod +x /usr/local/bin/deploy.sh
```
1. The `chmod +x` command adds execute permissions to the script for the user, group, and others.
2. Verify the permissions have been set correctly by listing the file details:
   ```bash
   ls -l /usr/local/bin/deploy.sh
   ```
3. The output should show that the script has execute permissions (e.g., `-rwxr-xr-x`).
4. Now, you can run the script using:
   ```bash
   /usr/local/bin/deploy.sh
   ```

