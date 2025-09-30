## Day 69: Install Jenkins Plugins

The DevOps team has recently setup a Jenkins server, which they want to use for some CI/CD jobs. Before that they want to install some plugins which will be used in most of the jobs. Please find below more details about the task
a. Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and Adm!n321 password.
b. Once logged in, install the Git and GitLab plugins. Note that you may need to restart Jenkins service to complete the plugins installation, If required, opt to Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre.

Note:
a. After restarting the Jenkins service, wait for the Jenkins login page to reappear before proceeding.
b. For tasks involving web UI changes, capture screenshots to share for review or consider using screen recording software like loom.com for documentation and sharing.

## Solution:

From jumphost login to the jenkins server using the credentials provided.

Go to the Manage Jenkins > Plugins Section > Search for Git and Gitlab Plugins > Install the plugins > restart the server.