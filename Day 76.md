## Day 76: Jenkins Project Security

The xFusionCorp Industries has recruited some new developers. There are already some existing jobs on Jenkins and two of these new developers need permissions to access those jobs. The development team has already shared those requirements with the DevOps team, so as per details mentioned below grant required permissions to the developers.

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.
There is an existing Jenkins job named Packages, there are also two existing Jenkins users named sam with password sam@pass12345 and rohan with password rohan@pass12345.
Grant permissions to these users to access Packages job as per details mentioned below:
a.) Make sure to select Inherit permissions from parent ACL under inheritance strategy for granting permissions to these users.
b.) Grant mentioned permissions to sam user : build, configure and read.
c.) Grant mentioned permissions to rohan user : build, cancel, configure, read, update and tag.

## Solution:

- Login to Jenkins using the provided credentials.
- Go to Manage Jenkins > Plugins > install "Matrix-based Authorization Strategy" plugin and restart Jenkins.
- Go to the configuration page of the Packages job.
- Select "Enable project-based security" and choose "Matrix-based security".
- Select "Inherit permissions from parent ACL" under inheritance strategy.
- Add users "sam" and "rohan" to the matrix.
- For user "sam", check the boxes for "Build", "Configure", and "Read".
- For user "rohan", check the boxes for "Build", "Cancel", "Configure", "Read", "Update", and "Tag".
- Save the configuration.

You have successfully granted the required permissions to the users for the Packages job.