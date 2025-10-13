## Day 81: Jenkins Multistage Pipeline

The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.
Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username sarah and password Sarah_pass123.
There is a repository named sarah/web in Gitea that is already cloned on Storage server under /var/www/html directory.
Update the content of the file index.html under the same repository to Welcome to xFusionCorp Industries and push the changes to the origin into the master branch.
Apache is already installed on all app Servers its running on port 8080.
Create a Jenkins pipeline job named deploy-job (it must not be a Multibranch pipeline job) and pipeline should have two stages Deploy and Test ( names are case sensitive ). Configure these stages as per details mentioned below.
a. The Deploy stage should deploy the code from web repository under /var/www/html on the Storage Server, as this location is already mounted to the document root /var/www/html of all app servers.
b. The Test stage should just test if the app is working fine and website is accessible. Its up to you how you design this stage to test it out, you can simply add a curl command as well to run a curl against the LBR URL (http://stlb01:8091) to see if the website is working or not. Make sure this stage fails in case the website/app is not working or if the Deploy stage fails.
Click on the App button on the top bar to see the latest changes you deployed. Please make sure the required content is loading on the main URL http://stlb01:8091 i.e there should not be a sub-directory like http://stlb01:8091/web etc.


## Solution:

- Go to Jenkins Dashboard and click on New Item.
- Install the Pipeline, Git, and any other required plugins if not already installed.
- Enter the name as deploy-job, select Pipeline and click OK.
- In the Pipeline section, select Pipeline script and paste the below code in the script box.
  ```groovy
    pipeline {
        agent any

        stages {
            stage('Deploy') {
                steps {
                    echo "Deploying ..."
                    git(
                        url: 'http://git.stratos.xfusioncorp.com/sarah/web.git',
                        branch: 'master',
                        credentialsId: 'GIT_CREDS'
                    )
                    // Copy files to web root (already mounted)
                    sh '''
                        cp -r * /var/www/html/
                    '''
                }
            }

            stage('Test') {
                steps {
                    echo "Testing deployment ..."
                    // Use curl to verify if the site is working
                    sh '''
                        STATUS=$(curl -s -o /dev/null -w "%{http_code}" http://stlb01:8091)
                        if [ "$STATUS" -ne 200 ]; then
                            echo "Site is not reachable. HTTP Status: $STATUS"
                            exit 1
                        fi
                    '''
                }
            }
        }
    }
  ```
- Click on Save to save the pipeline job.
- Click on Build Now to run the pipeline job.
- Monitor the console output to ensure both stages complete successfully.
- Finally, verify the deployment by accessing http://stlb01:8091 in a web browser to see the updated content "Welcome to xFusionCorp Industries".