One of the Nautilus DevOps team members was working to configure services on a kkloud container that is running on App Server 2 in Stratos Datacenter. Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:


a. Install apache2 in kkloud container using apt that is running on App Server 2 in Stratos Datacenter.


b. Configure Apache to listen on port 8085 instead of default http port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc.


c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.


------------------

Certainly, I'll provide you with the steps to complete the remaining tasks:

**Step 1: Access the Container**

1. SSH into App Server 2:
   ```bash
   ssh your_username@appserver2_ip
   ```

2. Access the kkloud container:
   ```bash
   docker exec -it kkloud /bin/bash
   ```

**Step 2: Install Apache2 and Configure**

3. Update the package list and install Apache2 using apt:
   ```bash
   apt update
   apt install apache2 -y
   ```

4. Edit the Apache configuration to listen on port 8085:
   ```bash
   echo "Listen 8085" > /etc/apache2/ports.conf
   ```

**Step 3: Configure Virtual Host**

5. Create a virtual host configuration file:
   ```bash
   echo "
   <VirtualHost *:8085>
       ServerAdmin webmaster@localhost
       DocumentRoot /var/www/html
       ErrorLog \${APACHE_LOG_DIR}/error.log
       CustomLog \${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>
   " > /etc/apache2/sites-available/000-default.conf
   ```

**Step 4: Start Apache Service**

6. Start the Apache service:
   ```bash
   service apache2 start
   ```

**Step 5: Keep the Container Running**

7. To keep the container running, you don't need to do anything special. The container will stay running as long as the process inside it is running. Since Apache is running as the main process, the container will stay active.

**Step 6: Verify Apache Configuration**

8. Verify that Apache is listening on port 8085 inside the container:
   ```bash
   curl http://localhost:8085
   ```

9. Exit the container:
   ```bash
   exit
   ```

10. If needed, you can start the container if it's not already running:
   ```bash
   docker start kkloud
   ```

That completes the tasks. Apache should now be installed and configured to listen on port 8085 inside the `kkloud` container running on App Server 2. The container will stay running as long as the Apache process is active.