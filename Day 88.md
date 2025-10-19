## Day 88: Ansible Blockinfile Module

The Nautilus DevOps team wants to install and set up a simple httpd web server on all app servers in Stratos DC. Additionally, they want to deploy a sample web page for now using Ansible only. Therefore, write the required playbook to complete this task. Find more details about the task below.
We already have an inventory file under /home/thor/ansible directory on jump host. Create a playbook.yml under /home/thor/ansible directory on jump host itself.
Using the playbook, install httpd web server on all app servers. Additionally, make sure its service should up and running.
Using blockinfile Ansible module add some content in /var/www/html/index.html file. Below is the content:
Welcome to XfusionCorp!
This is  Nautilus sample file, created using Ansible!
Please do not modify this file manually!
The /var/www/html/index.html file's user and group owner should be apache on all app servers.
The /var/www/html/index.html file's permissions should be 0777 on all app servers.


## Solution:

Create an inventory file:
  ```
  [appservers]
  stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_password=Ir0nM@n
  stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_password=Am3ric@
  stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_password=BigGr33n
  ```

create a playbook:
  ```yaml
  ---
  - name: Setup httpd and deploy sample webpage
    hosts: appservers
    become: yes
    tasks:
      - name: Install httpd package
        yum:
          name: httpd
          state: present

      - name: Ensure httpd service is running
        service:
          name: httpd
          state: started
          enabled: yes

      - name: Add content to index.html using blockinfile
        blockinfile:
          path: /var/www/html/index.html
          owner: apache
          group: apache
          mode: '0777'
          block: |
            Welcome to XfusionCorp!
            This is  Nautilus sample file, created using Ansible!
            Please do not modify this file manually!
  ```

Run the playbook

You have successfully set up the httpd web server and deployed the sample web page on all application servers using Ansible.