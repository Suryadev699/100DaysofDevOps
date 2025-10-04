## Day 73: Jenkins Scheduled Jobs

The devops team is working on to setup centralised logging management system to maintain and analyse server logs easily. Since it will take some time to implement, they wanted to gather some server logs on a regular basis. At least one of the app servers is having issues with the Apache server. The team needs Apache logs so that they can identify and troubleshoot the issues easily if they arise. So they decided to create a Jenkins job to collect logs from the server. Please create/configure a Jenkins job as per details mentioned below:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321
1. Create a Jenkins jobs named copy-logs.
2. Configure it to periodically build every 9 minutes to copy the Apache logs (both access_log and error_logs) from App Server 1 (from default logs location) to location /usr/src/data on Storage Server.

Note:
1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.
2. Please make sure to define you cron expression like this */10 * * * * (this is just an example to run job every 10 minutes).

## Solution:

- Login to Jenkins UI via the credentials provided in the instructions.
- Install necessary plugins like SSH plugin to enable SSH connections.
- Create a new job named 'copy-logs'.
- Enable Build periodically and set the cron expression to 'H/9 * * * *' to run the job every 9 minutes.
- Configure SSH credentials for the App server and Storage server in Jenkins credentials section.
- Also add SSH site for both servers in Jenkins system configuration.
- In the build steps, add a shell script to copy the Apache logs from App server 1 to the specified location on Storage Server.
```bash
echo Am3ric@ | sshpass -p Bl@kW scp -o StrictHostKeyChecking=no -r /var/log/httpd/* natasha@ststor01:/usr/src/data/
```
- Save the configuration and run the job to ensure it works as expected. 
- Click on build now to test the job and verify that logs are being copied. 

Submit the configuration and You now have a Jenkins job that runs periodically to copy Apache logs.
