## Day 77: Jenkins Deploy Pipeline

The development team is working on to develop a new static website and they are planning to deploy the same on App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.
Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username sarah and password Sarah_pass123. There under user sarah you will find a repository named web_app that is already cloned on Storage server under /var/www/html. sarah is a developer who is working on this repository.
Add a slave node named Storage Server. It should be labeled as ststor01 and its remote root directory should be /var/www/html.
We have already cloned repository on Storage Server under /var/www/html.
Apache is already installed on all app Servers its running on port 8080.
Create a Jenkins pipeline job named nautilus-webapp-job (it must not be a Multibranch pipeline) and configure it to:
Deploy the code from web_app repository under /var/www/html on Storage Server, as this location is already mounted to the document root /var/www/html of app servers. The pipeline should have a single stage named Deploy ( which is case sensitive ) to accomplish the deployment.
LB server is already configured. You should be able to see the latest changes you made by clicking on the App button. Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be a sub-directory like https://<LBR-URL>/web_app etc.

## Solution:

1. Access Jenkins UI by clicking on the Jenkins button on the top bar. Login using username `admin` and password `Adm!n321`.
2. Access Gitea UI by clicking on the Gitea button on the top bar. Login using username `sarah` and password `Sarah_pass123`. Under user `sarah`, you will find a repository named `web_app` that is already cloned on Storage server under `/var/www/html`.
3. Add a slave node named `Storage Server`:
   - Go to Jenkins dashboard.
   - Click on "Manage Jenkins" > "Manage Nodes and Clouds" > "New Node".
   - Enter the name `Storage Server`, select "Permanent Agent", and click "OK".
   - Configure the node:
     - Set the Remote root directory to `/var/www/html`.
     - Add the label `ststor01`.
     - Configure the launch method (e.g., via SSH) with the appropriate credentials.
   - Save the configuration.
4. Create a Jenkins pipeline job named `nautilus-webapp-job`:
   - From the Jenkins dashboard, click on "New Item".
   - Enter the name `nautilus-webapp-job`, select "Pipeline", and click "OK".
   - In the pipeline configuration:
     - Under "General", check "Restrict where this project can be run" and enter the label `ststor01`.
     - Under "Pipeline", select "Pipeline script" and enter the following script:
       ```groovy
       pipeline {
           agent { label 'ststor01' }
           stages {
               stage('Deploy') {
                   steps {
                       // Pull the latest code from the Gitea repository
                       git url: 'http://<GITEA-URL>/sarah/web_app.git', branch: 'master'
                       // Copy the code to the document root of the app servers
                       sh 'cp -r * /var/www/html/'
                   }
               }
           }
       }
       ```
     - Replace `<GITEA-URL>` with the actual URL of your Gitea server.
   - Save the pipeline configuration.
5. Run the pipeline job by clicking on "Build Now".
6. Verify the deployment by accessing the main URL of the load balancer (LBR-URL) to ensure the latest changes are reflected without any sub-directory.
7. If everything is set up correctly, you should see the static website deployed successfully.

You have now successfully created a Jenkins pipeline job to deploy the static website on the app servers.