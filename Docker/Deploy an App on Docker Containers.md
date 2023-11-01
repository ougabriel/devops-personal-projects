The Nautilus Application development team recently finished development of one of the apps that they want to deploy on a containerized platform. The Nautilus Application development and DevOps teams met to discuss some of the basic pre-requisites and requirements to complete the deployment. The team wants to test the deployment on one of the app servers before going live and set up a complete containerized stack using a docker compose fie. Below are the details of the task:



On App Server 3 in Stratos Datacenter create a docker compose file /opt/finance/docker-compose.yml (should be named exactly).


The compose should deploy two services (web and DB), and each service should deploy a container as per details below:


For web service:


a. Container name must be php_host.


b. Use image php with any apache tag. Check here for more details.


c. Map php_host container's port 80 with host port 6000


d. Map php_host container's /var/www/html volume with host volume /var/www/html.


For DB service:


a. Container name must be mysql_host.


b. Use image mariadb with any tag (preferably latest). Check here for more details.


c. Map mysql_host container's port 3306 with host port 3306


d. Map mysql_host container's /var/lib/mysql volume with host volume /var/lib/mysql.


e. Set MYSQL_DATABASE=database_host and use any custom user ( except root ) with some complex password for DB connections.


After running docker-compose up you can access the app with curl command curl <server-ip or hostname>:6000/



----------

To create a Docker Compose file for deploying two services (web and DB) on App Server 3 in the /opt/finance/ directory, follow these steps:

1. SSH into App Server 3.

2. Create a directory for the Docker Compose file if it doesn't exist:

   ```bash
   mkdir -p /opt/finance
   ```

3. Create the Docker Compose file `/opt/finance/docker-compose.yml` using a text editor. You can use `vi`, `nano`, or any other text editor you prefer. Here, we'll use `vi`:

   ```bash
   vi /opt/finance/docker-compose.yml
   ```

   Add the following contents to the `docker-compose.yml` file:

   ```yaml
   version: '3'
   services:
     web:
       container_name: php_host
       image: php:apache
       ports:
         - "6000:80"
       volumes:
         - /var/www/html:/var/www/html

     db:
       container_name: mysql_host
       image: mariadb:latest
       ports:
         - "3306:3306"
       volumes:
         - /var/lib/mysql:/var/lib/mysql
       environment:
         MYSQL_DATABASE: database_host
         MYSQL_USER: mysql-user
         MYSQL_PASSWORD: db1234
   ```

   Save and exit the text editor.

4. Start the containers using Docker Compose:

   ```bash
   docker-compose -f /opt/finance/docker-compose.yml up -d
   ```

   This command will launch the containers in the background.

5. After the containers are running, you can access the web service using `curl`. Replace `<server-ip or hostname>` with the actual IP or hostname of App Server 3:

   ```bash
   curl <server-ip or hostname>:6000/
   ```

   This should allow you to access the web application.

Make sure that you have Docker and Docker Compose installed on App Server 3 before proceeding with the Docker Compose file. Adjust the image and version tags as necessary based on your requirements.