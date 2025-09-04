## Day 21: Set Up Git Repository on Storage Server

Set up a Git Repository on your server to manage your project files efficiently.

1. **Install Git**:
    ```bash
    sudo yum install git
    ```
2. **Initialize a bare repository**:
    ```bash
    mkdir -p /opt/git/myproject.git
    cd /opt/git/myproject.git
    git init --bare
    ```

You now have a bare Git repository set up on your storage server. You can clone this repository to your local machine and push changes to it as needed.