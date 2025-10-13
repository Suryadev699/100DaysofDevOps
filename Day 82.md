## Day 82: Create Ansible Inventory for app server testing

The Nautilus DevOps team is testing Ansible playbooks on various servers within their stack. They've placed some playbooks under /home/thor/playbook/ directory on the jump host and now intend to test them on app server 1 in Stratos DC. However, an inventory file needs creation for Ansible to connect to the respective app. Here are the requirements:

a. Create an ini type Ansible inventory file /home/thor/playbook/inventory on jump host.
b. Include App Server 1 in this inventory along with necessary variables for proper functionality.
c. Ensure the inventory hostname corresponds to the server name as per the wiki, for example stapp01 for app server 1 in Stratos DC.

Note: Validation will execute the playbook using the command ansible-playbook -i inventory playbook.yml. Ensure the playbook functions properly without any extra arguments.


## Solution:

- In the jump host, create the inventory file using a text editor:
  ```bash
  nano /home/thor/playbook/inventory
  [appservers]
  stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_password=Ir0nM@n ansible_sudo_pass=Ir0nM@n
  ```
- Save and exit the file.
- Ensure the playbook runs successfully with the command:
  ```bash
  ansible-playbook -i inventory playbook.yml
  ```
- Verify that the playbook executes without errors and performs the intended tasks on App Server 1.

You have successfully created the Ansible inventory file and tested the playbook on App Server 1.