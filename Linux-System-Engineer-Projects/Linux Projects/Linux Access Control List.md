The Nautilus security team performed an audit on all servers present in Stratos DC. During the audit some critical data/files were identified which were having the wrong permissions as per security standards. Once the report was shared with the production support team, they started fixing the issues one by one. It has been identified that one of the files named /etc/resolv.conf on Nautilus App 1 server has wrong permissions, so that needs to be fixed and the correct ACLs needs to be set.



1. The user owner and group owner of the file should be root user.

2. Others must have read only permissions on the file.

3. User kirsty must not have any permission on the file.

4. User eric should have read only permission on the file.


--------------------


To correct the permissions and ACLs on the `/etc/resolv.conf` file on the Nautilus App 1 server according to the provided instructions, you can follow these steps:

1. **Change User and Group Ownership:**
   Change the owner and group of the file `/etc/resolv.conf` to `root`.

   ```bash
   sudo chown root:root /etc/resolv.conf
   ```

2. **Set File Permissions:**
   Set the file permissions to give read-only access to others and read-only access to the user `eric`.

   ```bash
   sudo chmod 644 /etc/resolv.conf
   ```

3. **Restrict Access for User `kirsty`:**
   Deny all permissions for the user `kirsty`.

   ```bash
   sudo setfacl -m u:kirsty:0 /etc/resolv.conf
   ```

4. **Grant Read-Only Access to User `eric`:**
   Grant read-only access to the user `eric`.

   ```bash
   sudo setfacl -m u:eric:r-- /etc/resolv.conf
   ```

5. **Verify ACLs:**
   Verify that the ACLs have been set correctly.

```bash
   getfacl /etc/resolv.conf
   ```

   The output should show the ACL entries for `root`, `kirsty`, and `eric`.

Please make sure to execute these commands with caution, as changing permissions and ACLs on system files can have serious consequences if not done correctly. Always test in a controlled environment before applying changes to production systems.

It's also recommended to have a backup of critical files before making changes, and to adhere to any organizational policies or procedures for handling server configuration changes.