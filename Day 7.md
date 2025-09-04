## Day 7: Linux SSH Authentication

This scenario is to set up SSH key-based authentication for a user on a Linux server.

The task here is to generate an SSH key pair on the client machine, copy the public key to the server, and configure the server to allow key-based authentication.

1. Generate an SSH key pair on the client machine (if you don't already have one):
   ```bash
   ssh-keygen -t rsa -b 2048
   ```
   Follow the prompts to save the key pair in the default location (`~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub`). You can choose to set a passphrase or leave it empty.
2. Copy the public key to the server:
   ```bash
   ssh-copy-id username@server_ip
   ```
   Replace `username` with your actual username on the server and `server_ip` with the server's IP address. You will be prompted to enter your password for the server.
3. Alternatively, you can manually copy the public key to the server:
   ```bash
   cat ~/.ssh/id_rsa.pub | ssh username@server_ip 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
   ```
4. Set the correct permissions on the server:
   ```bash
   ssh username@server_ip
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```
5. Test the SSH key-based authentication:
   ```bash
   ssh username@server_ip
   ```

If everything is set up correctly, you should be able to log in without being prompted for a password.