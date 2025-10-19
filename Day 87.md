## Day 87: Ansible Install Package

The Nautilus Application development team wanted to test some applications on app servers in Stratos Datacenter. They shared some pre-requisites with the DevOps team, and packages need to be installed on app servers. Since we are already using Ansible for automating such tasks, please perform this task using Ansible as per details mentioned below:
Create an inventory file /home/thor/playbook/inventory on jump host and add all app servers in it.
Create an Ansible playbook /home/thor/playbook/playbook.yml to install samba package on all  app servers using Ansible yum module.
Make sure user thor should be able to run the playbook on jump host.

## Solution:

- create an inventory :
  ```
  [appservers]
  stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_password=Ir0nM@n
  stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_password=Am3ric@
  stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_password=BigGr33n
  ```
- create a playbook :
  ```yaml
  ---
  - name: Install samba
    hosts: appservers
    become: yes
    tasks:
      - name: Install samba package
        yum:
          name: samba
          state: present
  ```
- run the playbook using ```bash ansible-playbook -i inventory playbook.yml``` & verify using the command ```bash ansible all -a "rpm -q samba" -i inventory```.

You have successfully installed the samba package on all application servers using Ansible.