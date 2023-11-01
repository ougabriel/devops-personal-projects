On Nautilus storage server in Stratos DC, there is a storage location named /data, which is used by different developers to keep their data (non confidential data). One of the developers named siva has raised a ticket and asked for a copy of their data present in /data/siva directory on storage server. /home is a FTP location on storage server itself where developers can download their data. Below are the instructions shared by the system admin team to accomplish this task.



a. Make a siva.tar.gz compressed archive of /data/siva directory and move the archive to /home directory on Storage Server.



----------------


To accomplish the task of creating a compressed archive of the `/data/siva` directory and moving it to the `/home` directory on the Nautilus storage server in the Stratos DC, you can follow these steps:

1. **Log in to the Storage Server:**

   SSH into the storage server using the appropriate credentials.

   ```bash
   ssh your_username@storage_server_ip
   ```

2. **Create a Compressed Archive:**

   Navigate to the `/data` directory and create a compressed archive of the `siva` directory using the `tar` command.

   ```bash
   cd /data
   tar -czvf /home/siva.tar.gz siva
   ```

   This command creates a compressed archive named `siva.tar.gz` of the `siva` directory and places it in the `/home` directory.

3. **Verify Archive Creation:**

   Verify that the compressed archive `siva.tar.gz` has been created in the `/home` directory.

   ```bash
   ls /home/siva.tar.gz
   ```

4. **Ensure File Permissions:**

   Make sure that the permissions of the archive are appropriate for the user to download it. You might need to adjust the permissions if necessary.

   ```bash
   chmod 644 /home/siva.tar.gz
   ```

5. **Inform the Developer:**

   Let the developer "siva" know that their data is now available in the compressed archive at `/home/siva.tar.gz` for them to download.

6. **Optional Cleanup (if needed):**

   After confirming that the developer has successfully downloaded the data, you can optionally remove the compressed archive.

   ```bash
   rm /home/siva.tar.gz
   ```

Please be cautious when working with file operations, especially when using commands like `rm`. Always test in a controlled environment before applying changes in production to avoid accidental data loss.

If there are any specific security or organization policies in place, make sure to adhere to them while handling data and file transfers.