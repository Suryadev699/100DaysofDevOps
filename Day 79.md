## Day 79: Jenkins Deployment Job

The development team had a meeting with the DevOps team where they discussed automating the deployment of one of their apps using Jenkins (the one in Stratos Datacenter). They want to auto deploy the new changes in case any developer pushes to the repository. As per the requirements mentioned below configure the required Jenkins job.
Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and Adm!n321 password.
Similarly, you can access the Gitea UI using Gitea button, username and password for Git is sarah and Sarah_pass123 respectively. Under user sarah you will find a repository named web that is already cloned on the Storage server under sarah's home. sarah is a developer who is working on this repository.
1. Install httpd (whatever version is available in the yum repo by default) and configure it to serve on port 8080 on All app servers. You can make it part of your Jenkins job or you can do this step manually on all app servers.
2. Create a Jenkins job named app-deployment and configure it in a way so that if anyone pushes any new change to the origin repository in master branch, the job should auto build and deploy the latest code on the Storage server under /var/www/html directory. Since /var/www/html on Storage server is shared among all apps.
Before deployment, ensure that the ownership of the /var/www/html directory is set to user sarah, so that Jenkins can successfully deploy files to that directory.
3. SSH into Storage Server using sarah user credentials mentioned above. Under sarah user's home you will find a cloned Git repository named web. Under this repository there is an index.html file, update its content to Welcome to the xFusionCorp Industries, then push the changes to the origin into master branch. This push must trigger your Jenkins job and the latest changes must be deployed on the servers, also make sure it deploys the entire repository content not only index.html file.
Click on the App button on the top bar to access the app, you should be able to see the latest changes you deployed. Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be any sub-directory like https://<LBR-URL>/web etc.

## Solution:

- Go to Jenkins and install plugins like gitea, Build Authorization Token Root, SSH, Publish over SSH.
- Create credentials for users of all appservers and sarah user of storage server. Since the credential is same for gitea and storage server, you can use the same credential.
- Create SSH remote hosts/ssh sites under the configuration for all app servers.
- Configure Publish over ssh plugin with the following details:
  - Name: StorageServer
  - Hostname: <Storage-Server-IP>
  - Username: sarah
  - Remote Directory: /var/www/html
  - Credentials: Add credentials for user sarah
- Create a new freestyle job to install httpd on all app servers and deploy the code on storage server.
  - name: httpd-install
  - in the build section, add a build step to execute shell command with the following script:
  ```bash
  echo Ir0nM@n | sudo -S sudo yum install -y httpd 
  echo Ir0nM@n | sudo -S sed -i 's/^Listen 80$/Listen 8080/g' /etc/httpd/conf/httpd.conf
  echo Ir0nM@n | sudo -S systemctl restart httpd && systemctl status httpd
  ```
  - Repeat for the other app servers.
  ```bash
  echo Am3ric@ | sudo -S sudo yum install -y httpd
  echo Am3ric@ | sudo -S sed -i 's/^Listen 80$/Listen 8080/g' /etc/httpd/conf/httpd.conf
  echo Am3ric@ | sudo -S systemctl restart httpd && systemctl status httpd
  ```
  ```bash
  echo BigGr33n | sudo -S sudo yum install -y httpd
  echo BigGr33n | sudo -S sed -i 's/^Listen 80$/Listen 8080/g' /etc/httpd/conf/httpd.conf
  echo BigGr33n | sudo -S systemctl restart httpd && systemctl status httpd
  ```
- Build the job and verify httpd is installed and running on port 8080 on all app servers.
- Create a new freestyle job named app-deployment with the following configuration:
  - In the Source Code Management section, select Git and provide the following details:
    - Repository URL: http://git.stratos.xfusioncorp.com/sarah/web.git
    - Credentials: Add credentials for user sarah
    - Branches to build: */master
  - In the Build Triggers section, select "Trigger builds remotely" and provide an authentication token (e.g., nautilus).
  - Copy the webhook url provided in the build trigger section. {"https://8080-port-koqnpuqih74azcn7.labs.kodekloud.com/buildByToken/build?job=nautilus-app-deployment&token=nautilus"}
  - In the Build Environment, select send files or execute commands over SSH and provide the following details:
    - Name: StorageServer
    - Source files: ** (to copy all files)
    - Remove prefix: web (to remove the web directory prefix)
    - Exec command: chown -R sarah:sarah /var/www/html (to change ownership of the directory)
- Login to the gitea ui and navigate to web repository settings. Under the Webhooks section, add a new webhook with the following details:
  - Target URL: Paste the webhook url copied from Jenkins job.
  - Content type: application/json
  - Secret: (leave blank)
  - Which events would you like to trigger this webhook?: Just the push event.
  - Active: Checked
- Save the webhook and test it to ensure it's working.
- Now, SSH into the storage server using sarah user credentials. Navigate to the web repository and update the index.html file with the following content:
  ```html
  <html>
    <head>
      <title>Welcome to xFusionCorp Industries</title>
    </head>
    <body>
      <h1>Welcome to the xFusionCorp Industries</h1>
    </body>
  </html>
  ```
- Commit and push the changes to the master branch:
  ```bash
  git add index.html
  git commit -m "Updated index.html"
  git push origin master
  ```
- This push should trigger the Jenkins job and deploy the latest code to /var/www/html on the storage server.
- Finally, click on the App button on the top bar to access the app.

You should be able to see the latest changes you deployed. Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be any sub-directory like https://<LBR-URL>/web etc.