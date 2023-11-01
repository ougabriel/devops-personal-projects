To stick with the security compliances, the Nautilus project team has decided to apply some restrictions on crontab access so that only allowed users can create/update the cron jobs. Limit crontab access to below specified users on App Server 1.



Allow crontab access to kareem user and deny the same to ryan user.

-------------


To allow crontab access to the `kareem` user and deny it to the `ryan` user on App Server 1, you can use the `/etc/cron.allow` and `/etc/cron.deny` files. These files control access to the `crontab` command.

Here's what you need to do:

1. **Allow Crontab Access for `kareem` User:**

   Create or edit the `/etc/cron.allow` file and add the `kareem` user to it. This will allow the `kareem` user to use the `crontab` command.

   ```bash
   sudo echo "kareem" > /etc/cron.allow
   ```

2. **Deny Crontab Access for `ryan` User:**

   Create or edit the `/etc/cron.deny` file and add the `ryan` user to it. This will deny the `ryan` user from using the `crontab` command.

   ```bash
   sudo echo "ryan" > /etc/cron.deny
   ```

Make sure to replace `kareem` and `ryan` with the actual usernames.

Remember that these changes affect only the `crontab` command and do not restrict the user from running other scheduled tasks using other mechanisms like cron jobs specified in system-wide cron directories (e.g., `/etc/cron.d`).


----------

Incase of error on cron.deny use the following steps


To correctly manage `cron.allow` and `cron.deny` files, please follow these steps:

1. **Add User to `cron.allow`:**

   To allow the `kareem` user to access `crontab`, you should add their username to the `/etc/cron.allow` file. Use the `echo` command with `sudo` to achieve this:

   ```bash
   sudo sh -c 'echo "kareem" >> /etc/cron.allow'
   ```

2. **Create or Edit `cron.deny` (if needed):**

   If you want to deny access to the `ryan` user, you need to create the `/etc/cron.deny` file if it doesn't exist, and add the `ryan` user to it. Use these commands:

   ```bash
   sudo touch /etc/cron.deny  # Create the file if it doesn't exist
   sudo sh -c 'echo "ryan" >> /etc/cron.deny'
   ```


------------

Verify your configuration

To verify the status of the `crontab` access for specific users, you can follow these steps:

1. **Check `cron.allow` and `cron.deny` Files:**

   The `cron.allow` and `cron.deny` files control access to the `crontab` command. You can inspect these files to see which users are allowed or denied.

   - To check `cron.allow`:
     ```bash
     cat /etc/cron.allow
     ```
     If this file exists, it lists users who are allowed to use the `crontab` command.

   - To check `cron.deny`:
     ```bash
     cat /etc/cron.deny
     ```
     If this file exists, it lists users who are denied access to the `crontab` command.

2. **Check User's `crontab` Access:**

   You can also check if a specific user has access to their own `crontab`.

   - To check `kareem` user's `crontab` access:
     ```bash
     sudo -u kareem crontab -l
     ```
     If this command shows the user's cron jobs, it indicates that the user has access to their `crontab`.

   - To check `ryan` user's `crontab` access:
     ```bash
     sudo -u ryan crontab -l
     ```
     If this command returns an error indicating that the user is not allowed, it confirms that the user's `crontab` access is restricted.

Re


