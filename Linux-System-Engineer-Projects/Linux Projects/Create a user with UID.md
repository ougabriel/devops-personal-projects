For security reasons the xFusionCorp Industries security team has decided to use custom Apache users for each web application hosted, rather than its default user. This will be the Apache user, so it shouldn't use the default home directory. Create the user as per requirements given below:



a. Create a user named anita on the App server 3 in Stratos Datacenter.


b. Set its UID to 1096 and home directory to /var/www/anita.



-------------



To create a user named "anita" with a specific UID and home directory on App Server 3 in the Stratos Datacenter, follow these steps:

1. **Connect to App Server 3:**

   Log in to App Server 3 using SSH or your preferred remote access method.

2. **Create the User:**

   Run the following command to create the "anita" user with the specified UID and home directory:

   ```bash
   sudo useradd -u 1096 -d /var/www/anita -m anita
   ```

   Explanation of the options used:
   - `-u 1096`: Set the UID to 1096.
   - `-d /var/www/anita`: Set the home directory to `/var/www/anita`.
   - `-m`: Create the home directory if it doesn't exist.

3. **Set Password for the User (Optional):**

   If you want to set a password for the "anita" user, you can use the `passwd` command:

   ```bash
   sudo passwd anita
   ```

   Follow the prompts to set the password.

4. **Confirm User Creation:**

   You can confirm that the user "anita" has been created by running:

   ```bash
   id anita
   ```

   This command should display information about the "anita" user, including the UID, home directory, and groups.

