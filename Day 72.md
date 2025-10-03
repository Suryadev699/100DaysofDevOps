## Day 72: Jenkins Parameterized Builds

A new DevOps Engineer has joined the team and he will be assigned some Jenkins related tasks. Before that, the team wanted to test a simple parameterized job to understand basic functionality of parameterized builds. He is given a simple parameterized job to build in Jenkins. Please find more details below:
Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.
a. Create a parameterized job which should be named as parameterized-job
b. Add a string parameter named Stage; its default value should be Build.
c. Add a choice parameter named env; its choices should be Development, Staging and Production.
d. Configure job to execute a shell command, which should echo both parameter values (you are passing in the job).
e. Build the Jenkins job at least once with choice parameter value Staging to make sure it passes

## Solution:

- Login to the Jenkins using the credentials provided.
- Click on New Item > Name it as parameterized-job > Select freestyle project > Click OK.
- Check the box "This project is parameterized".
- Click on Add parameter > Select String Parameter.
    - Name: Stage
    - Default Value: Build
- Click on Add Parameter > Select Choice Parameter.
    - Name: env
    - Choices:
        ```
        Development
        Staging
        Production
        ```
- Scroll down to Build Section > Click Add build step > Select Execute Shell.
- In the command box, enter:
    ```bash
    echo "$Stage" "$env"
    ```
- Click Save.
- Click on Build with parameters. Choose the necessary values and click on Build.

You have successfully created a parameterized Jenkins job.