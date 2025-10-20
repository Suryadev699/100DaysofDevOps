## Day 89: Ansible Managed Service

Developers are looking for dependencies to be installed and run on Nautilus app servers in Stratos DC. They have shared some requirements with the DevOps team. Because we are now managing packages installation and services management using Ansible, some playbooks need to be created and tested. As per details mentioned below please complete the task:
a. On jump host create an Ansible playbook /home/thor/ansible/playbook.yml and configure it to install vsftpd on all app servers.
b. After installation make sure to start and enable vsftpd service on all app servers.
c. The inventory /home/thor/ansible/inventory is already there on jump host.
d. Make sure user thor should be able to run the playbook on jump host.

## Solution:

create a playbook:
  ```yaml
  ---
  - name: Install and manage vsftpd service
    hosts: appservers
    become: yes
    tasks:
      - name: Install vsftpd package
        yum:
          name: vsftpd
          state: present

      - name: Ensure vsftpd service is running and enabled
        service:
          name: vsftpd
          state: started
          enabled: yes
  ```
Run the playbook using the command:
  ```bash
  ansible-playbook -i /home/thor/ansible/inventory /home/thor/ansible/playbook.yml
  ```

You have successfully installed and managed the vsftpd service on all application servers using Ansible.