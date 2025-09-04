## Day 10: Linux Bash Scripts

The scenaro involves creating a bashscript to automate the backup of a directory. The goal is to create a script that compresses a specified directory and saves it to a backup location with a timestamp.

1. **Create the Bash Script**:
    Open a terminal and create a new bash script file.

    ```bash
    nano backup.sh
    ```
2. **Write the Script**:
    Add the following lines to the script:
    ```bash
    #!/bin/bash

    # Set variables
    source_dir="set as given in the scenario"
    archive_name="set as given in the scenario"
    backup_dir_<servername>="/backup"
    backup_dir_<backup server>="/backup"

    # Create the archive
    echo "Creating archive..."
    zip -r "$backup_dir_<servername>/$archive_name" "$source_dir"

    # Check for successful archive creation
    if [[ $? -eq 0 ]]; then
    echo "Archive created successfully: $backup_dir_<servername>/$archive_name"
    else
    echo "Error creating archive. Please check logs."
    exit 1
    fi

    # Copy the archive to Nautilus Backup Server (using SSH key-based authentication)
    echo "Copying archive to Nautilus Backup Server..."
    scp -o StrictHostKeyChecking=no "$backup_dir_<servername>/$archive_name" user@<nautilus_server_ip>:"$backup_dir_nautilus"

    # Check for successful copy
    if [[ $? -eq 0 ]]; then
    echo "Archive copied successfully to Nautilus Backup Server."
    else
    echo "Error copying archive. Please check SSH connection and permissions."
    exit 1
    fi

    echo "Backup completed."
    ```

3. **Make the Script Executable**:
    Save the file and exit the editor. Then, make the script executable.
    ```bash
    chmod +x backup.sh
    ```

Now execute the script to perform the backup operation.