## Day 70: Configure Jenkins User access

The DevOps team is integrating Jenkins into their CI/CD pipelines. After setting up a new Jenkins server, they're now configuring user access for the development team, Follow these steps:
a. Click on the Jenkins button on the top bar to access the Jenkins UI. Login with username admin and password Adm!n321.
b. Create a jenkins user named mark with the passwordB4zNgHA7Ya. Their full name should match Mark.
c. Utilize the Project-based Matrix Authorization Strategy to assign overall read permission to the mark user.
d. Remove all permissions for Anonymous users (if any) ensuring that the admin user retains overall Administer permissions.
e. For the existing job, grant mark user only read permissions, disregarding other permissions such as Agent, SCM etc.

## Solution:

From jumphost login to the jenkins server using the credentials provided.

Go to the Manage Jenkins > Users

Add the user with the details provided. Click Save.

Next go to the Plugins > Install Matrix-based Project Authorization Plugin and restart Jenkins.

Now go to Manage Jenkins > Security > Authorisation > Matrix-based Project Authorization Strategy.

A dropdown window appears to fine tune permission to specific users. Add the Mark User and Check the Overall Read Permission.

For the Existing Job, Go to the Job > Configure > Enable Project Based Authorization. Add user to the dropdown menu that appears. Tick Read Permission. Save it.

You now have defined the User Access in Jenkins.