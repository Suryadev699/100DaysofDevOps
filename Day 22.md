## Day 22: Clone Git Repository on Storage Server

This taks is to clone a Git repositoru from Github to a local server.

1. Clone the repository:
    ```bash
    git clone /opt/demo.git /usr/src/myprojectrepo
    ```
2. Verify the cloned repository:
    ```bash
    ls /usr/src/myprojectrepo
    ```
3. Check the Git status:
    ```bash
    cd /usr/src/myprojectrepo
    git status
    ```

You now have a cloned Git repository on your local server.