On our Storage server in Stratos Datacenter we are having some issues where nfsuser user is holding hundred of processes, which is degrading the performance of the server. Therefore, we have a requirement to limit its maximum processes. Please set its maximum process limits as below:



a. soft limit = 1026


b. hard_limit = 2027


------------------


To set the maximum process limits for the `nfsuser` user on your Storage server in the Stratos Datacenter, you can use the `ulimit` command in conjunction with the user-specific resource limits configuration file. Here's how you can achieve this:

1. **Edit the User-Specific Resource Limits Configuration:**

   Navigate to the user-specific resource limits configuration directory:

   ```bash
   cd /etc/security/limits.d/
   ```

   Create or edit a file, for example, `nfsuser-limits.conf`, to define the process limits for the `nfsuser` user:

   ```bash
   sudo nano nfsuser-limits.conf
   ```

   Add the following lines to the file:

   ```
   nfsuser soft nproc 1026
   nfsuser hard nproc 2027
   ```

   Save the file and exit the editor.

2. **Apply the Limits:**

   After saving the file, the new limits will take effect the next time the `nfsuser` user logs in. To apply them without requiring a user log out and log in, you can use the `ulimit` command:

   ```bash
   ulimit -u 1026
   ```

   This will set the soft limit for the current session of the `nfsuser` user.

Please note that the `ulimit` changes will only affect the current shell session, while the user-specific resource limits configuration will apply to future sessions. It's recommended to apply the limits using both methods to ensure that the limits are consistently applied.

