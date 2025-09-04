## Day 11: Install and Configure Tomcat Server

This scenario involves installing and configuring the Apache Tomcat server on a app server instance.

1. **Install Tomcat**:
    ```bash
    sudo yum install tomcat -y
    ```

2. **Change the port settings**:
    - Open the Tomcat configuration file:
      ```bash
      sudo vi /etc/tomcat/server.xml
      ```
    - Change the default port (8080) to 9090 by modifying the following line:
      ```xml
      <Connector port="9090" protocol="HTTP/1.1"
                 connectionTimeout="20000"
                 redirectPort="8443" />
      ```

3. **Copy the ROOT.war file**:
    - The ROOT.war file is located in the '/tmp' directory on jump host.
    - Copy the ROOT.war file to the Tomcat webapps directory:
      ```bash
      sudo cp /tmp/ROOT.war /var/lib/tomcat/webapps/
      ```

4. **Start and enable the Tomcat service**:
    ```bash
    sudo systemctl start tomcat
    sudo systemctl enable tomcat
    ```

This completes the installation and configuration of the Tomcat server on the app server instance. You can now access the Tomcat server on port 9090.

Note: Ensure to change the port number as directed in the instructions.