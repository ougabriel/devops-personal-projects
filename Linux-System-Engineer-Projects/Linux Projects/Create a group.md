There are specific access levels for users defined by the xFusionCorp Industries system admin team. Rather than providing access levels to every individual user, the team has decided to create groups with required access levels and add users to that groups as needed. See the following requirements:



a. Create a group named nautilus_admin_users in all App servers in Stratos Datacenter.


b. Add the user stark to nautilus_admin_users group in all App servers. (create the user if doesn't exist).



-------------


Certainly, I can guide you through the steps to accomplish these tasks. However, please note that I can only provide general instructions as I don't have direct access to your systems. Make sure to adapt these instructions to your environment as needed.

1. **Create a Group named "nautilus_admin_users":**

   SSH into each of the App servers in the Stratos Datacenter and follow these steps:

   a. Log in to the server using SSH:
   ```bash
   ssh your_username@app_server_ip
   ```

   b. Add the group "nautilus_admin_users":
   ```bash
   sudo groupadd nautilus_admin_users
   ```

2. **Add User "stark" to the Group "nautilus_admin_users":**

   If the user "stark" doesn't exist, you'll need to create the user first:

   a. Create the user "stark" (if not already created):
   ```bash
   sudo useradd stark
   ```

   b. Set a password for the user "stark":
   ```bash
   sudo passwd stark
   ```

   c. Add the user "stark" to the group "nautilus_admin_users":
   ```bash
   sudo usermod -aG nautilus_admin_users stark
   ```

   d. Verify the user's groups:
   ```bash
   groups stark
   ```

   The output should include the "nautilus_admin_users" group.

Repeat these steps on all the App servers within the Stratos Datacenter.

Please be cautious while making changes to user and group configurations, as incorrect changes can potentially disrupt the system. Always test changes in a controlled environment before implementing them in production.




----------------


