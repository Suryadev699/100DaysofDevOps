## Day 6: Create a Cron Job

This scenario is to create a cron job that runs a script every day at midnight.

The task here is to install the cron package, create a script that logs the current date and time to a file, and set up a cron job to execute this script daily at midnight.

1. Install the cron package if it's not already installed:
   ```bash
   sudo apt-get install cron
   ```

2. Create a script that logs the current date and time to a file:
   ```bash
   sudo nano /usr/local/bin/log_date.sh
   ```
   Add the following lines to the script:
   ```bash
   #!/bin/bash
   date >> /var/log/date.log
   ```
   Save and exit the editor.

3. Make the script executable:
   ```bash
   sudo chmod +x /usr/local/bin/log_date.sh
   ```

4. Set up a cron job to execute the script daily at midnight:
   ```bash
   sudo crontab -e
   ```
   Add the following line to the crontab file:
   ```
   0 0 * * * /usr/local/bin/log_date.sh
   ```
   Save and exit the editor.

5. Verify that the cron job has been added:
   ```bash
   sudo crontab -l
   ```
6. Ensure the cron service is running:
   ```bash
   sudo service cron start
   ```

7. Test the cron job by running:
   ```bash
   sudo /usr/local/bin/log_date.sh
   ```
   Check the log file to see if the date has been logged:
   ```bash
   cat /var/log/date.log
   ```


This way we can ensure that the cron job is set up correctly and will log the date and time every day at midnight.