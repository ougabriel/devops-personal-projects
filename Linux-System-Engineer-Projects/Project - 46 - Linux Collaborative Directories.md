The Nautilus team doesn't want its data to be accessed by any of the other groups/teams due to security reasons and want their data to be strictly accessed by the dbadmin group of the team.



Setup a collaborative directory /dbadmin/data on Nautilus App 1 server in Stratos Datacenter.

The directory should be group owned by the group dbadmin and the group should own the files inside the directory. The directory should be read/write/execute to the group owners, and others should not have any access.



To set up a collaborative directory `/dbadmin/data` on Nautilus App 1 server in the Stratos Datacenter with the specified permissions, you can follow these steps:

1. Log in to the Nautilus App 1 server using SSH or any other preferred method.
2. Navigate to the root directory by running the following command:
   ```
   cd /
   ```
3. Create the `/dbadmin/data` directory by running the following command:
   ```
   sudo mkdir -p dbadmin/data
   ```
   This command creates the `dbadmin` directory if it doesn't exist, and then creates the `data` directory inside it.
4. Set the group ownership to `dbadmin` for the `/dbadmin/data` directory:
   ```
   sudo chown :dbadmin dbadmin/data
   ```
5. Set the directory permissions to read/write/execute for the group owners:
   ```
   sudo chmod 770 dbadmin/data
   ```
   This command sets the read/write/execute permissions for the group owners, while removing all access for others.
6. Set the group ownership and permissions recursively for the files inside the directory:
   ```
   sudo chgrp -R dbadmin dbadmin/data
   sudo chmod -R g=rwX dbadmin/data
   ```
   The first command (`chgrp`) sets the group ownership to `dbadmin` recursively for all files and directories inside `/dbadmin/data`. The second command (`chmod`) sets the group permissions to read and write (`rw`) and also sets the execute (`X`) permission for directories. This ensures that all files and directories within `/dbadmin/data` are owned by the `dbadmin` group and have the appropriate permissions.

Now, the `/dbadmin/data` directory and its contents are set up with the desired group ownership and permissions. Only the `dbadmin` group members will have read, write, and execute access, while others will have no access to the directory.

