## Day 78: Jenkins Conditional Pipeline

The development team is working on to develop a new static website and they are planning to deploy the same on App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:
Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.
Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username sarah and password Sarah_pass123. There under user sarah you will find a repository named web_app that is already cloned on Storage server under /var/www/html. sarah is a developer who is working on this repository.
Add a slave node named Storage Server. It should be labeled as ststor01 and its remote root directory should be /var/www/html.
We have already cloned repository on Storage Server under /var/www/html.
Apache is already installed on all app Servers its running on port 8080.
Create a Jenkins pipeline job named xfusion-webapp-job (it must not be a Multibranch pipeline) and configure it to:
Add a string parameter named BRANCH.
It should conditionally deploy the code from web_app repository under /var/www/html on Storage Server, as this location is already mounted to the document root /var/www/html of app servers. The pipeline should have a single stage named Deploy ( which is case sensitive ) to accomplish the deployment.
The pipeline should be conditional, if the value master is passed to the BRANCH parameter then it must deploy the master branch, on the other hand if the value feature is passed to the BRANCH parameter then it must deploy the feature branch.
LB server is already configured. You should be able to see the latest changes you made by clicking on the App button. Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be a sub-directory like https://<LBR-URL>/web_app etc.


## Solution:

- Go to Jenkins dashboard and install necessary plugins like pipeline, git, ssh build agents etc
- Create a new node with the following details:
  - Name: Storage Server
  - Remote root directory: /var/www/html
  - Labels: ststor01
  - Launch method: Launch agent via SSH
  - Host: <Storage-Server-IP>
  - Credentials: Add credentials for user sarah
- Create a new pipeline job named xfusion-webapp-job with the following configuration:
```groovy
pipeline {
    agent {
        label 'ststor01'
    }
    
    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description: 'Branch to deploy (master or feature)')
    }
    
    stages {
        stage('Deploy') {
            when {
                expression {
                    params.BRANCH == 'master' || params.BRANCH == 'feature'
                }
            }
            steps {
                script {
                    def repositoryPath = '/var/www/html/'

                    if (params.BRANCH == 'master') {
                        git branch: 'master',
                            url: 'http://git.stratos.xfusioncorp.com/sarah/web_app.git'
                    } else if (params.BRANCH == 'feature') {
                        git branch: 'feature',
                            url: 'http://git.stratos.xfusioncorp.com/sarah/web_app.git'
                    }
                    
                    sh "cp -r /var/www/html/workspace/xfusion-webapp-job/* /var/www/html/"
                }
            }
        }
    }
}
```
- Save the job and run it with the parameter BRANCH set to either master or feature.
- Verify the deployment by accessing the URL https://<LBR-URL> and ensure the content is loading correctly without any sub-directory.

You have successfully created a Jenkins conditional pipeline to deploy the static website on App Servers.