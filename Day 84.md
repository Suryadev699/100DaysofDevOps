## Day 84: Copy Data to App Servers using Ansible

The Nautilus DevOps team needs to copy data from the jump host to all application servers in Stratos DC using Ansible. Execute the task with the following details:
a. Create an inventory file /home/thor/ansible/inventory on jump_host and add all application servers as managed nodes.
b. Create a playbook /home/thor/ansible/playbook.yml on the jump host to copy the /usr/src/itadmin/index.html file to all application servers, placing it at /opt/itadmin.
Note: Validation will run the playbook using the command ansible-playbook -i inventory playbook.yml. Ensure the playbook functions properly without any extra arguments.

## Solution:

- Create a inventory file at /home/thor/ansible/inventory with the details:
    ```
    [appservers]
    stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_password=Ir0nM@n
    stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_password=Am3ric@
    stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_password=BigGr33n
    ```
- Create a playbook at /home/thor/ansible/playbook.yml with the following content:
    ```yaml
    ---
    - hosts: appservers
      become: true
      tasks:
        - name: Create the /opt/itadmin directory
          file:
            path: /opt/itadmin
            state: directory
            mode: '0755' # Set appropriate permissions
          become: true

        - name: Copy index.html to application servers
          copy:
            src: /usr/src/itadmin/index.html
            dest: /opt/itadmin/index.html
            mode: '0644' # Set appropriate permissions
        become: true
    ```
- Run the playbook using the command mentioned in the instructions.

You have successfully copied the index.html file to all application servers in the specified directory using Ansible.
