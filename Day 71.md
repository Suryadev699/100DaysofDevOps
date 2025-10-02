## Day 71: Configure Jenkins Job for Package Installation

Some new requirements have come up to install and configure some packages on the infrastructure under Stratos Datacenter. The DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:
a. Access the Jenkins UI by clicking on the Jenkins button in the top bar. Log in using the credentials: username admin and password Adm!n321.
b. Create a new Jenkins job named install-packages and configure it with the following specifications:
   Add a string parameter named PACKAGE.
   Configure the job to install a package specified in the $PACKAGE parameter on the storage server within the Stratos Datacenter.


## Solution:

Login to the Jenkins UI using the credentials provided.

Install required plugins if not already installed. For this task, we will need the "SSH" plugin to connect to the remote server and execute commands.

After installing the plugin, restart Jenkins if prompted.

Create a new credentials entry for SSH access to the storage server:
- Go to "Manage Jenkins" > "Manage Credentials".
- Select the Global domain (or appropriate domain).
- Click on "Add Credentials".
- Choose "Username and password" as the kind.
- Enter the username and password of the storage server.
- Click on Save.

Create a new SSH Site under "manage jenkins" > "Configure System":
- Scroll down to the "SSH remote hosts" section.
- Add new site with the detials of storage server and test connection.
- Once connection is successful, save the configuration.

Now, create a new Jenkins Job:
- Click on "New Item" in the Jenkins dashboard.
- Enter "install-packages" as the name of the job.
- Select "freestyle" Click ok.
- Inside the configuration page, scroll down to select "This project is parameterized" and add a "String Parameter" named "PACKAGE".
- Scroll down to the "Build" section and click on "Add build step".
- Select "Send files or execute commands over SSH".
- Select the SSH site created earlier.
- In the "Exec command" field, enter the command to install the package using the parameter:
  ```bash
  sudo yum install -y $PACKAGE
  ```
- Save the job configuration.

Now, you can run the job by clicking on "Build with Parameters" and entering the package name you want to install on the storage server.

You now have a Jenkins job that can install packages on the storage server by specifying the package name as a parameter.