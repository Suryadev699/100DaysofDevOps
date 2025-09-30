## Day 68: Set up Jenkins Server

The DevOps team is initiating the setup of CI/CD pipelines and has decided to utilize Jenkins as their server. Execute the task according to the provided requirements:
a. Install Jenkins on the jenkins server using the yum utility only, and start its service.
b. Jenkin's admin user name should be theadmin, password should be Adm!n321, full name should be Mark and email should be mark@jenkins.stratos.xfusioncorp.com.
Note:
a. To access the jenkins server, connect from the jump host using the root user with the password S3curePass.
b. After Jenkins server installation, click the Jenkins button on the top bar to access the Jenkins UI and follow on-screen instructions to create an admin user.

## Solution:

From jumphost login to the jenkins server using the credentials provided.

Go to the official Jenkins documentaion website, search the distribution in which jenkins needs to be installed and follow the steps:

**Important**: As instructed the installation should be done using the *yum* utility only.

Steps:
```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io-2023.key
sudo yum upgrade
# Add required dependencies for the jenkins package
sudo yum install fontconfig java-21-openjdk
sudo yum install jenkins
```
This commands will install the dependencines and Jenkins on the Jenkins server.

Now start the jenkins server,

You can enable the Jenkins service to start at boot with the command:
```bash
sudo systemctl enable jenkins
```

You can start the Jenkins service with the command:
```bash
sudo systemctl start jenkins
```

You can cehck the status of the Jenkins service with the command:
```bash
sudo systemctl status jenkins
```

If everything has been set up correctly, you should see an output like this:
```bash
Loaded: loaded (/lib/systemd/system/jenkins.service; enabled; vendor preset: enabled)
Active: active (running) since Tue 2025-09-30 16:19:01 +03; 4min 57s ago
...
```
Now click on the Jenkins button on the top bar. This will take you to the setup page.

When you first access a new Jenkins controller, you are asked to unlock it using an automatically-generated password.

you can find the password at,
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

This will print the password. Use it to proceed for next steps, installating the recommended plugins and packages, create the admin user with the details as provided. 

You now have successfully installed and configured the Jenkins server.
