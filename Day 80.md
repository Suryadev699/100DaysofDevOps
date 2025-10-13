## Day 80: Jenkins Chained Builds

The DevOps team was looking for a solution where they want to restart Apache service on all app servers if the deployment goes fine on these servers in Stratos Datacenter. After having a discussion, they came up with a solution to use Jenkins chained builds so that they can use a downstream job for services which should only be triggered by the deployment job. So as per the requirements mentioned below configure the required Jenkins jobs.

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and Adm!n321 password.
Similarly you can access Gitea UI on port 8090 and username and password for Git is sarah and Sarah_pass123 respectively. Under user sarah you will find a repository named web.
Apache is already installed and configured on all app server so no changes are needed there. The doc root /var/www/html on all these app servers is shared among the Storage server under /var/www/html directory.

1. Create a Jenkins job named nautilus-app-deployment and configure it to pull change from the master branch of web repository on Storage server under /var/www/html directory, which is already a local git repository tracking the origin web repository. Since /var/www/html on Storage server is a shared volume so changes should auto reflect on all apps.
2. Create another Jenkins job named manage-services and make it a downstream job for nautilus-app-deployment job. Things to take care about this job are:
  a. This job should restart httpd service on all app servers.
  b. Trigger this job only if the upstream job i.e nautilus-app-deployment is stable.

LB server is already configured. Click on the App button on the top bar to access the app. You should be able to see the latest changes you made. Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be a sub-directory like https://<LBR-URL>/web etc.


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
- Create a new upstreamjob named nautilus-app-deployment with the following configuration:
  - In the Source Code Management section, select Git and provide the following details:
    - Repository URL: http://git.stratos.xfusioncorp.com/sarah/web.git
    - Credentials: Add credentials for user sarah
    - Branches to build: */master
  - In the Build Environment, select send files or execute commands over SSH and provide the following details:
    - Name: StorageServer
    - Source files: ** (to copy all files)
    - Remove prefix: web (to remove the web directory prefix)
    - Exec command: chown -R sarah:sarah /var/www/html (to change ownership of the directory)
  - In the Build Triggers section, select "Trigger builds remotely" and provide an authentication token (e.g., nautilus).
  - Copy the webhook url provided in the build trigger section. {"https://8080-port-d2ydxbosb64cadtx.labs.kodekloud.com/buildByToken/build?job=nautilus-app-deployment&token=nautilus"}
  - Build the job and verify the code is deployed on storage server under /var/www/html directory.
  - Login to the gitea ui and navigate to web repository settings. Under the Webhooks section, add a new webhook with the following details:
    - Target URL: Paste the webhook url copied from Jenkins job.
    - Content type: application/json
    - Secret: (leave blank)
    - Which events would you like to trigger this webhook?: Just the push event.
    - Active: Checked
    - Test the delivery and verify the webhook is working fine.
- Create a new downstream job named manage-services with the following configuration:
  - In the Build Triggers section, select "Build after other projects are built" and provide the following details:
    - Projects to watch: nautilus-app-deployment
    - Trigger only if build is stable.
  - In the Build Environment, select send files or execute commands over SSH and provide the following details:
    - Name: AppServers (the ssh site created for all app servers)
    - Exec command:
    ```bash
    echo Ir0nM@n | sudo -S systemctl restart httpd && systemctl status httpd
    ```
    - Repeat the step for remaining app servers
    ```bash
    echo Am3ric@ | sudo -S systemctl restart httpd && systemctl status httpd
    ```
    ```bash
    echo BigGr33n | sudo -S systemctl restart httpd && systemctl status httpd
    ```
- Save the job and test the downstream job by building the upstream job.
- Click on the App button on the top bar to access the app, you should be able to see the latest changes you deployed. Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be any sub-directory like https://<LBR-URL>/web etc.

You have successfully completed the Jenkins chained builds scenario.