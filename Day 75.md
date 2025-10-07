## Day 75: Jenkins Slave Nodes

The DevOps team has installed and configured new Jenkins server in Stratos DC which they will use for CI/CD and for some automation tasks. There is a requirement to add all app servers as slave nodes in Jenkins so that they can perform tasks on these servers using Jenkins. Find below more details and accomplish the task accordingly.

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.
1. Add all app servers as SSH build agent/slave nodes in Jenkins. Slave node name for app server 1, app server 2 and app server 3 must be App_server_1, App_server_2, App_server_3 respectively.
2. Add labels as below:
App_server_1 : stapp01
App_server_2 : stapp02
App_server_3 : stapp03
3. Remote root directory for App_server_1 must be /home/tony/jenkins, for App_server_2 must be /home/steve/jenkins and for App_server_3 must be /home/banner/jenkins.
4. Make sure slave nodes are online and working properly.

## Solution:

Prerequisite is to install jdk on all app servers.

- Login to the Jenkins server using the credentials.
- Go to Manage Jenkins > Manage Nodes > New Node.
- Enter the node name as App_server_1, select "Permanent Agent" and click OK.
- In the configuration, enter the following details:
  - Description: App Server 1
  - # of executors: 2
  - Remote root directory: /home/tony/jenkins
  - Labels: stapp01
  - Launch method: Launch agents via SSH
  - Host: stapp01.stratos.xfusioncorp.com
  - Credentials: Add new credentials with username tony and password Ir0nM@n
- Click "Test Connection" to verify the connection.
- Save the configuration.
- Repeat the above steps for App_server_2 and App_server_3 with their respective details.
- After adding all nodes, ensure they are online and connected properly.

You have successfully added all app servers as Jenkins slave nodes.