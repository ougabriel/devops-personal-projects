During a recent security audit, the application security team of xFusionCorp Industries found security issues with the Apache web server on Nautilus App Server 3 server in Stratos DC. They have listed several security issues that need to be fixed on this server. Please apply the security settings below:



a. On Nautilus App Server 3 it was identified that the Apache web server is exposing the version number. Ensure this server has the appropriate settings to hide the version number of the Apache web server.

b. There is a website hosted under /var/www/html/blog on App Server 3. It was detected that the directory /blog lists all of its contents while browsing the URL. Disable the directory browser listing in Apache config.

c. Also make sure to restart the Apache service after making the changes.

To apply the security settings for Apache web server on Nautilus App Server 3, please follow the steps below:

a. Hide the Apache web server version number:
   1. Open the Apache configuration file using a text editor. The location of the file may vary, but it is commonly found at `/etc/httpd/conf/httpd.conf` or `/etc/apache2/apache2.conf`.
   2. Look for the line that starts with `ServerTokens`. By default, it may be set to `ServerTokens OS`.
   3. Change the value to `ServerTokens Prod` to hide the Apache version number.
   4. Save the changes and exit the text editor.

b. Disable directory browser listing:
   1. Open the Apache configuration file mentioned in the previous step.
   2. Look for the `<Directory /var/www/html>` section (or a similar directory section) that corresponds to the `/var/www/html` directory.
   3. Inside that section, locate the line containing `Options Indexes FollowSymLinks`.
   4. Remove `Indexes` from the line so that it becomes `Options FollowSymLinks`.
   5. Save the changes and exit the text editor.

c. Restart the Apache service:
   - Use the appropriate command for your operating system:
     - For CentOS/RHEL: `sudo systemctl restart httpd`
     - For Ubuntu/Debian: `sudo systemctl restart apache2`

After performing these steps, the Apache web server on Nautilus App Server 3 will have the appropriate security settings applied.