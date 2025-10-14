## Day 83: Troubleshoot and Create ansible playbook

An Ansible playbook needs completion on the jump host, where a team member left off. Below are the details:
The inventory file /home/thor/ansible/inventory requires adjustments. The playbook must run on App Server 2 in Stratos DC. Update the inventory accordingly.
Create a playbook /home/thor/ansible/playbook.yml. Include a task to create an empty file /tmp/file.txt on App Server 2.

## Solution:

1. Correct the inventory file /home/thor/ansible/inventory to ensure it includes App Server 2 in Stratos DC with the correct details:
   ```bash
   stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_common_args='-o StrictHostKeyChecking=no' ansible_password=Am3ric@
   ```
2. Create the playbook /home/thor/ansible/playbook.yml with the following content:
   ```yaml
   ---
   - name: Create an empty file on App Server 2
     hosts: stapp02
     tasks:
       - name: Create an empty file /tmp/file.txt
         file:
           path: /tmp/file.txt
           state: touch
    ```
3. Run the playbook from the jump host:
   ```bash
   ansible-playbook -i /home/thor/ansible/inventory /home/thor/ansible/playbook.yml
   ```

You have successfully created the playbook and executed it to create an empty file on App Server 2.
