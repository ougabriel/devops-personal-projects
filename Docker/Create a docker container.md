Nautilus DevOps team is testing some applications deployment on some of the application servers. They need to deploy a nginx container on Application Server 2. Please complete the task as per details given below:


On Application Server 2 create aDOCKER container named nginx_2 using image nginx with alpine tag and make sure container is in running state.

To deploy an Nginx container named `nginx_2` using the `nginx` image with the `alpine` tag on Application Server 2, follow these steps:

1. **Connect to Application Server 2:**

   Log in to Application Server 2 using SSH or your preferred remote access method.

2. **Pull and Run Nginx Container:**

   Run the following command to pull and run the Nginx container using the specified image and tag:

   ```bash
   docker run -d --name nginx_2 -p 80:80 nginx:alpine
   ```

   Explanation of the options used:
   - `-d`: Run the container in detached mode (background).
   - `--name nginx_2`: Assign the name `nginx_2` to the container.
   - `-p 80:80`: Map port 80 from the host to port 80 in the container.
   - `nginx:alpine`: Use the `nginx` image with the `alpine` tag.

3. **Check Container Status:**

   You can check the status of the container using the following command:

   ```bash
   docker ps -a --filter "name=nginx_2"
   ```

   This will display information about the `nginx_2` container, including its status.
