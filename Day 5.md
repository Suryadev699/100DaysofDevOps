## Day 5: SElinux Installation and Configuration

This scenario focuses on installing and configuring Security-Enhanced Linux (SElinux) on a Linux system. SElinux provides an additional layer of security by enforcing access control policies that restrict how processes can interact with each other and with files.

The task is to install SElinux, enable it, and configure it to enforce security policies.

1. Install SElinux packages using the package manager. For example, on a Red Hat-based system, you can use:
   ```bash
   sudo yum install selinux-policy selinux-policy-targeted
   ```
   On a Debian-based system, use:
   ```bash
   sudo apt-get install selinux-basics selinux-policy-default
   ```
2. Enable SElinux by editing the configuration file located at `/etc/selinux/config`. Change the line `SELINUX=disabled` to `SELINUX=enforcing`:
   ```bash
   sudo nano /etc/selinux/config
   ```
   Modify the file to look like this:
   ```
   SELINUX=enforcing
   SELINUXTYPE=targeted
   ```
3. Reboot the system to apply the changes:
   ```bash
   sudo reboot
   ```
4. After rebooting, verify that SElinux is enabled and enforcing by running:
   ```bash
   sestatus
   ```

The output should indicate that SElinux is enabled and in enforcing mode.