During the daily standup, it was pointed out that the timezone across Nautilus Application Servers in Stratos Datacenter doesn't match with that of the local datacenter's timezone, which is America/Managua.



Correct the mismatch.


To correct the timezone mismatch across Nautilus Application Servers in the Stratos Datacenter, follow these steps:

1. SSH into each application server using the appropriate credentials.
2. Open the timezone configuration file using a text editor. The file is usually located at `/etc/timezone` or `/etc/localtime`.
3. Update the timezone setting to "America/Managua" in the configuration file.
4. Save the changes and exit the text editor.
5. Restart the application server or services for the changes to take effect.

Repeat these steps for each Nautilus Application Server in the Stratos Datacenter to ensure consistent timezone settings across all servers.


or

To correct the timezone mismatch across Nautilus Application Servers in the Stratos Datacenter running CentOS, you can follow these steps:

1. SSH into each application server using the appropriate credentials.
2. Check the current timezone setting by running the command `timedatectl show --property=Timezone`.
or
timedatectl status | grep Timezone

3. If the timezone is different from "America/Managua", proceed to the next step. Otherwise, no further action is needed.
4. Set the timezone to "America/Managua" by running the command `sudo timedatectl set-timezone America/Managua`.
5. Verify the updated timezone by running `timedatectl show --property=Timezone` again.
6. Repeat these steps for each Nautilus Application Server in the Stratos Datacenter to ensure consistent timezone settings across all servers.

After making the changes, the timezone across all the CentOS servers should be corrected to "America/Managua".

OR 

If you are not getting any output when running the `timedatectl status | grep Timezone` command, it means that the timezone information is not available or there might be an issue with the command.

In CentOS, you can check the timezone by examining the symbolic link `/etc/localtime`. Please try the following command:

```
ls -l /etc/localtime
```

This command will show the symlink pointing to the currently set timezone file.

If the symlink points to a different timezone than "America/Managua", you can update it by creating a new symlink. Run the following command to set the correct timezone:

```
sudo ln -sf /usr/share/zoneinfo/America/Managua /etc/localtime
```

After creating the symlink, you can verify the updated timezone by running the `ls -l /etc/localtime` command again.

Repeat these steps for each Nautilus Application Server in the Stratos Datacenter running CentOS to ensure consistent timezone settings across all servers.