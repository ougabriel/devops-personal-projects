One of the DevOps team members was working on to create a new custom docker image on App Server 1 in Stratos DC. He is done with his changes and image is saved on same server with name official:datacenter. Recently a requirement has been raised by a team to use that image for testing, but the team wants to test the same on App Server 3. So we need to provide them that image on App Server 3 in Stratos DC.


a. On App Server 1 save the image official:datacenter in an archive.


b. Transfer the image archive to App Server 3.


c. Load that image archive on App Server 3 with same name and tag which was used on App Server 1.



-----------------------



To transfer a Docker image from one server to another, you can follow these steps. In this example, we'll use the `docker save` command to save the image to an archive, transfer the archive to App Server 3, and then load the image from the archive on App Server 3.

On **App Server 1** (source server):

a. Save the Docker image `official:datacenter` to an archive:

```bash
docker save -o official_datacenter.tar official:datacenter
```

This command will create an archive file named `official_datacenter.tar` containing the Docker image.

b. Transfer the image archive to **App Server 3** (destination server):

You can use various methods to transfer the archive to App Server 3, such as SCP, SFTP, or a shared file system. Here's an example using SCP:

```bash
scp official_datacenter.tar user@AppServer3IP:/path/to/destination/
```

Replace `user` with your username, `AppServer3IP` with the IP address of App Server 3, and `/path/to/destination/` with the desired path on App Server 3 where you want to copy the archive.

On **App Server 3** (destination server):

c. Load the Docker image from the archive with the same name and tag:

```bash
docker load -i official_datacenter.tar
```

This command will load the Docker image from the `official_datacenter.tar` archive, and it should be available on App Server 3 with the same name and tag as it had on App Server 1 (`official:datacenter`).

Now, the Docker image should be ready for testing on **App Server 3** as requested by the testing team.

-----
To confirm that the Docker image has been successfully loaded on the target server (in this case, **App Server 3**), you can use the `docker images` command. Here's how to do it:

1. SSH into **App Server 3** if you're not already logged in.

2. Run the following command to list all the Docker images:

```bash
docker images
```

This command will display a list of all Docker images available on **App Server 3**. Look for the image with the name and tag you expect (e.g., `official:datacenter`) in the list.

The output of the `docker images` command will include columns like `REPOSITORY`, `TAG`, `IMAGE ID`, and `CREATED`. Make sure the image you loaded appears in this list with the correct repository name and tag.

If you see the image in the list, it means the image has been successfully loaded and is ready to use on **App Server 3**.