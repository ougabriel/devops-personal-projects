The Nautilus application development team shared static website content that needs to be hosted on the httpd web server using a containerised platform. The team has shared details with the DevOps team, and we need to set up an environment according to those guidelines. Below are the details:



a. On App Server 3 in Stratos DC create a container named httpd using a docker compose file /opt/docker/docker-compose.yml (please use the exact name for file).


b. Use httpd (preferably latest tag) image for container and make sure container is named as httpd; you can use any name for service.


c. Map 80 number port of container with port 5001 of docker host.


d. Map container's /usr/local/apache2/htdocs volume with /opt/security volume of docker host which is already there. (please do not modify any data within these locations).

-------

To set up the environment as per the provided guidelines and verify that the Docker Compose for the `httpd` container is running, follow these steps on App Server 3 (banner@172.238.16.12):

**Step 1:** SSH into App Server 3

```bash
ssh banner@172.238.16.12
```

Use your SSH username and password when prompted.

**Step 2:** Create a Docker Compose file

Create a Docker Compose file named `/opt/docker/docker-compose.yml` with the following content:

```yaml
version: '3'
services:
  httpd:
    image: httpd:latest
    container_name: httpd
    ports:
      - "5001:80"
    volumes:
      - /opt/security:/usr/local/apache2/htdocs
```

You can create and edit this file using a text editor like `nano` or `vim`.

**Step 3:** Start the Docker Compose

Once you have created the Docker Compose file, you can start the container using the following command:
```bash
docker-compose -f /opt/docker/docker-compose.yml up -d
```

OR

Navigate to the directory where the Docker Compose file is located:

```bash
cd /opt/docker/
```

Start the Docker Compose service:

```bash
docker-compose up -d
```

This command will start the `httpd` container defined in the Docker Compose file in detached mode.

**Step 4:** Confirm Docker Compose is Running

Use the following `curl` command to check if the web server served by the Docker Compose is running:

```bash
curl http://localhost:5001
```

If you see the web page content in the terminal, it means the Docker Compose setup for the `httpd` container is running successfully.

That's it! You've created the `httpd` container using Docker Compose on App Server 3 and verified its functionality using the `curl` command. The web server should be accessible at http://localhost:5001.