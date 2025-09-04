## Day 17: Install and Configure PostgreSQL

We have a requirement from the operations team to set up a PostgreSQL database server on our app server. The database will be used to store data from our web applications. 

Task is to install PostgreSQL, create a new database and user, configure the permission to the database.

1. **Install PostgreSQL**:
    ```bash
    suod yum update -y && sudo yum install -y postgresql-server postgresql-contrib
    ```
2. Login to the PostgreSQL shell:
    ```bash
    sudo -u postgres psql
    ```
3. Create a new database:
    ```sql
    CREATE DATABSE <NAME AS SPECIFIED>;
    ```
4. Create a new user with a password:
    ```sql
    CREATE USER <USERNAME AS SPECIFIED> WITH ENCRYPTED PASSWORD '<PASSWORD AS SPECIFIED>';
    ```
5. Grant all privileges on the database to the user:
    ```sql
    GRANT ALL PRIVILEGES ON DATABASE <NAME AS SPECIFIED> TO <USERNAME AS SPECIFIED>;
    ```
6. Exit the PostgreSQL shell:
    ```sql
    \q
    ```
7. Start and enable PostgreSQL service:
    ```bash
    sudo systemctl start postgresql
    sudo systemctl enable postgresql
    ```
8. Verify the installation and connection:
    ```bash
    psql -U <USERNAME AS SPECIFIED> -d <NAME AS SPECIFIED>
    ```

9. Exit the PostgreSQL shell:
    ```sql
    \q
    ```

