## Day 86: Ansible Ping module usage

The Nautilus DevOps team is planning to test several Ansible playbooks on different app servers in Stratos DC. Before that, some pre-requisites must be met. Essentially, the team needs to set up a password-less SSH connection between Ansible controller and Ansible managed nodes. One of the tickets is assigned to you; please complete the task as per details mentioned below:


a. Jump host is our Ansible controller, and we are going to run Ansible playbooks through thor user from jump host.


b. There is an inventory file /home/thor/ansible/inventory on jump host. Using that inventory file test Ansible ping from jump host to App Server 2, make sure ping works.

## Solution:

first generate a ssh key: ssh-keygen -t rsa -b 4096
copy the key to the host: ssh-copy-id -i ~/.ssh/id_rsa.pub <server_username>@<server-address>
now test using ssh.
after successful update the inventory file to see any errors & execute. (in this case there is no entry for the ansible_user so added)
ansible stapp02 -m ping -i inventory