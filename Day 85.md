## Day 85: Create Files on App Servers using Ansible

The Nautilus DevOps team is testing various Ansible modules on servers in Stratos DC. They're currently focusing on file creation on remote hosts using Ansible. Here are the details:


a. Create an inventory file ~/playbook/inventory on jump host and include all app servers.


b. Create a playbook ~/playbook/playbook.yml to create a blank file /home/appdata.txt on all app servers.


c. Set the permissions of the /home/appdata.txt file to 0744.


d. Ensure the user/group owner of the /home/appdata.txt file is tony on app server 1, steve on app server 2 and banner on app server 3.


## Solution:

- Create an inventory:
    ```
    [appservers]
    stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_password=Ir0nM@n
    stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_password=Am3ric@
    stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_password=BigGr33n
    ```
- create a playbook:
  ```yaml
    ---
    - name: create new files
    hosts: all
    become: yes
    tasks:
    - name: create and set file permission
        file:
        path: /home/appdata.txt
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
        mode: "0755"
        state: touch
    ```
- run the playbook using ```bash ansible-playbook -i inventory playbook.yml``` & verify using the command ```bash ansible all -a "ls -lsd /home/appdata.txt" -i inventory```.

You have successfully created the /home/appdata.txt file on all application servers with the specified permissions and ownership using Ansible.
