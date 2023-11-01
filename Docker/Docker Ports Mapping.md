The Nautilus DevOps team is planning to host an application on a nginx-based container. There are number of tickets already been created for similar tasks. One of the tickets has been assigned to set up a nginx container on Application Server 1 in Stratos Datacenter. Please perform the task as per details mentioned below:


a. Pull nginx:alpine-perl docker image on Application Server 1.


b. Create a container named games using the image you pulled.


c. Map host port 8087 to container port 80. Please keep the container in running state.


---------------

To set up an Nginx container on Application Server 1 in Stratos Datacenter, follow these steps:

**Step 1:** SSH into Application Server 1

```bash
ssh your_username@Application_Server_1_IP
```

Replace `your_username` with your actual SSH username and `Application_Server_1_IP` with the server's IP address. Provide your password when prompted.

**Step 2:** Pull the Nginx Docker Image

Use the following command to pull the `nginx:alpine-perl` Docker image:

```bash
docker pull nginx:alpine-perl
```

This command will download the specified Docker image from Docker Hub to your server.

**Step 3:** Create a Container

Now, create a container named "games" using the Nginx image you just pulled. Map host port 8087 to container port 80. Run the container in the background (`-d` flag):

```bash
docker run -d --name games -p 8087:80 nginx:alpine-perl
```

This command creates a container named "games" using the Nginx image and maps port 8087 on the host to port 80 in the container. The container will run in detached mode.

**Step 4:** Verify the Container

You can verify that the container is running by using the following command:

```bash
docker ps
```

This command will show a list of running containers, and you should see the "games" container in the list.

The Nginx container is now running on Application Server 1, mapped to port 8087, and serving web content. You can access the Nginx web server by opening a web browser and navigating to `http://Application_Server_1_IP:8087`, where `Application_Server_1_IP` is the IP address of your server.