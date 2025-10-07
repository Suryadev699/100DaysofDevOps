## Day 74: Jenkins Database Backup Job

There is a requirement to create a Jenkins job to automate the database backup. Below you can find more details to accomplish this task:
Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.
Create a Jenkins job named database-backup.
Configure it to take a database dump of the kodekloud_db01 database present on the Database server in Stratos Datacenter, the database user is kodekloud_roy and password is asdfgdsd.
The dump should be named in db_$(date +%F).sql format, where date +%F is the current date.
Copy the db_$(date +%F).sql dump to the Backup Server under location /home/clint/db_backups.
Further, schedule this job to run periodically at */10 * * * * (please use this exact schedule format).


## Solution:

- Login to Jenkins and install ssh plugin
- enable "build this periodically" and set the value to "*/10 * * * *"
- in the build steps select "execute shell" and add the below script
```bash
# Create database dump with current date in filename
sshpass -p Sp!dy ssh -o StrictHostKeyChecking=no peter@stdb01 "mysqldump -u kodekloud_roy -pasdfgdsd kodekloud_db01" > db_$(date +%F).sql

# Copy the dump file to backup server
sshpass -p H@wk3y3 scp -o StrictHostKeyChecking=no db_$(date +%F).sql clint@stbkp01:/home/clint/db_backups/
```
- Save the job and click "Build now".

You have successfully created a backup Job