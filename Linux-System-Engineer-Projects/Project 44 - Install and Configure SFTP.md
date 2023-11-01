TASK 45 - Install And Configure SFTP


Some of the developers from Nautilus project team have asked for SFTP access to at least one of the app server in Stratos DC. After going through the requirements, the system admins team has decided to configure the SFTP server on App Server 2 server in Stratos Datacenter. Please configure it as per the following instructions:



a. Create an SFTP user mark and set its password to LQfKeWWxWD.

b. Password authentication should be enabled for this user.

c. Set its ChrootDirectory to /var/www/nfsshare.

d. SFTP user should only be allowed to make SFTP connections.

ChatGPT





To configure the SFTP server on App Server 2 in the Stratos Datacenter according to the given instructions, follow these steps:


ADDITIONAL

- Create a directory and set its ChrootDirectory
```
mkdir -p /var/www/nfsshare
```
- confirm the directory
```
ll -lsd /var/www/nfsshare
```
- change ownership 
```
chown root:root /var/www
```
- change the permission of the dir to read, write and executable permissions
```
chmod -R 755 /var/www
```


1. Create the SFTP user 'mark' and set its password to 'LQfKeWWxWD':
   ```bash
   sudo useradd mark
   sudo passwd mark
   # Enter the password 'LQfKeWWxWD' when prompted
   ```

2. Enable password authentication for the 'mark' user by editing the SSH server configuration file. Open the file `/etc/ssh/sshd_config` using a text editor:
   ```bash
   sudo vi /etc/ssh/sshd_config
   ```

3. Locate the following line in the file and ensure it is uncommented (remove any '#' character at the beginning):
   ```
   #PasswordAuthentication yes
   ```

   Change it to:
   ```
   PasswordAuthentication yes
   ```
4. Set the ChrootDirectory for the 'mark' user to '/var/www/nfsshare'. Add the following lines at the end of the SSH server configuration file: (it should exactly like this, comment on the sftp    /usr/libexec/openssh/sftp-server else it will throw error and the user will still be able to use ssh )
   ```
#subsystem      sftp    /usr/libexec/openssh/sftp-server
Subsystem       sftp    internal-sftp

Match User anita

ForceCommand internal-sftp

PasswordAuthentication yes

ChrootDirectory /var/www/nfsshare

PermitTunnel no

AllowTcpForwarding no

X11Forwarding no

AllowAgentForwarding no
   ```

   This configuration restricts the 'mark' user to the specified directory, allows only SFTP connections, enables password authentication, and disables TCP forwarding and X11 forwarding.

5. Save the changes and exit the text editor.

6. Restart the SSH service for the changes to take effect:
   ```bash
   sudo systemctl restart sshd
   ```

After completing these steps, the SFTP server should be configured on App Server 2, and the 'mark' user should be able to connect using SFTP with the specified restrictions.
