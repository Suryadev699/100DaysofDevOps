## Day 3: Secure Root SSH Access

This scenario focuses on enhancing the security of SSH access to the root account on a Linux server. By default, many Linux distributions allow root login via SSH, which can be a significant security risk. Disabling root SSH access and using key-based authentication can help mitigate this risk.

```bash
sudo nano /etc/ssh/sshd_config
```
1. Locate the line that says `PermitRootLogin` and change its value to `no`:
   ```
   PermitRootLogin no
   ```
2. Ensure that `PasswordAuthentication` is set to `no` to enforce key-based authentication:
   ```
   PasswordAuthentication no
   ```
3. Save the file and exit the editor.

4. Restart the SSH service to apply the changes:
   ```bash
   sudo systemctl restart sshd
   ```

