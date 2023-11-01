
The Nautilus DevOps team is testing applications containerization, which issupposed to be migrated on docker container-based environments soon. In today's stand-up meeting one of the team members has been assigned a task to create and test a docker container with certain requirements. Below are more details:


a. On App Server 1 in Stratos DC pull nginx image (preferably latest tag but others should work too).


b. Create a new container with name apps from the image you just pulled.


c. Map the host volume /opt/dba with container volume /usr/src/. There is an sample.txt file present on same server under /tmp; copy that file to /opt/dba. Also please keep the container in running state


------------
To accomplish the tasks mentioned, you can follow these steps on App Server 1:

1. Pull the latest Nginx image from Docker Hub (assuming you have Docker installed):

   ```bash
   docker pull nginx:latest
   ```

   This command will download the latest Nginx image to your local Docker repository.

2. Create a new Docker container with the name "apps" using the Nginx image:

   ```bash
   docker run -d --name apps -v /opt/dba:/usr/src/ nginx:latest
   ```

   - `-d`: Run the container in detached mode.
   - `--name apps`: Assign the name "apps" to the container.
   - `-v /opt/dba:/usr/src/`: Map the host volume `/opt/dba` to the container volume `/usr/src/`.
   - `nginx:latest`: Use the Nginx image with the latest tag.

3. Copy the `sample.txt` file from `/tmp` on the host to `/opt/dba` in the container. You can use the `docker cp` command to copy the file:

   ```bash
   docker cp /tmp/sample.txt apps:/usr/src/sample.txt
   ```

   This command copies the `sample.txt` file from the host to the `/usr/src/` directory inside the "apps" container.

4. Verify that the container is running:

   ```bash
   docker ps
   ```

   This command will list all running containers, and you should see the "apps" container in the list.

The Nginx container named "apps" is now running, and the `/opt/dba` directory on the host is mapped to `/usr/src/` in the container. The `sample.txt` file from the host has been copied into the container.