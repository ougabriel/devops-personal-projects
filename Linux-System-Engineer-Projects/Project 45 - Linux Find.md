During a routine security audit, the team identified an issue on the Nautilus App Server. Some malicious content was identified within the website code. After digging into the issue they found that there might be more infected files. Before doing a cleanup they would like to find all similar files and copy them to a safe location for further investigation. Accomplish the task as per the following requirements:



a. On App Server 2 at location /var/www/html/blog find out all files (not directories) having .css extension.

b. Copy all those files along with their parent directory structure to location /blog on same server.

c. Please make sure not to copy the entire /var/www/html/blog directory content.



To accomplish the task, you can use the following commands:

a. To find all files with the .css extension in the specified location:
```
find /var/www/html/blog -type f -name "*.css"
```

b. To copy the files and their parent directory structure to the specified location:
```
find /var/www/html/blog -type f -name "*.css" -exec cp --parents {} /blog \;
```

These commands will find all files with the .css extension in the `/var/www/html/blog` directory and its subdirectories. Then, it will copy those files to the `/blog` directory on the same server while preserving their parent directory structure.

Please note that you should run these commands with appropriate permissions (e.g., using sudo) to ensure proper access and write permissions for the files and directories involved.