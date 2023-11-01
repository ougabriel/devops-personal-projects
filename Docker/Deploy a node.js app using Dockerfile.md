There is a requirement to Dockerize a Node app and to deploy the same on App Server 3. Under /node_app directory on App Server 3, we have already placed a package.json file that describes the app dependencies and server.js file that defines a web app framework.



Create a Dockerfile (name is case sensitive) under /node_app directory:

Use any node image as the base image.
Install the dependencies using package.json file.
Use server.js in the CMD.
Expose port 5001.

The build image should be named as nautilus/node-web-app.


Now run a container named nodeapp_nautilus using this image.

Map the container port 5001 with the host port 8096.

. Once deployed, you can test the app using a curl command on App Server 3:



curl http://localhost:8096


----

To Dockerize your Node.js app and deploy it on App Server 3, you can follow these steps: 

```bash
ssh banner@172.16.238.12
```

1. Navigate to the `/node_app` directory on App Server 3.

2. Create a Dockerfile named `Dockerfile` (case sensitive) with the following content:

```Dockerfile
# Use an official Node.js runtime as the base image
FROM node:14

# Set the working directory in the container
WORKDIR /app

# Copy the package.json and package-lock.json to the container
COPY package*.json ./

# Install the app dependencies
RUN npm install

# Copy the rest of the application code to the container
COPY . .

# Expose port 5001
EXPOSE 5001

# Define the command to run your Node.js application
CMD ["node", "server.js"]
```

3. Build the Docker image with the name `nautilus/node-web-app`. Run the following command in the `/node_app` directory:

```bash
docker build -t nautilus/node-web-app .
```

4. Once the image is successfully built, you can run a container named `nodeapp_nautilus` using the image and map the container's port 5001 to the host's port 8096:

```bash
docker run -d -p 8096:5001 --name nodeapp_nautilus nautilus/node-web-app
```

Here's a breakdown of the command:
- `-d`: Runs the container in detached mode.
- `-p 8096:5001`: Maps host port 8096 to container port 5001.
- `--name nodeapp_nautilus`: Names the container as `nodeapp_nautilus`.
- `nautilus/node-web-app`: Specifies the image to use.

5. Your container is now running, and your Node.js app should be accessible on port 8096. You can test it using the `curl` command as instructed:

```bash
curl http://localhost:8096
```

This command will send a request to your Node.js application running in the container, and you should receive a response.

That's it! You've successfully Dockerized your Node.js app and deployed it as a container on App Server 3.