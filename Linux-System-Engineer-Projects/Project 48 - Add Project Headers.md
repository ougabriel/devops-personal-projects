TASK 48 - ADD RESPONSE HEADERS IN APACHE

We are working on hardening Apache web server on all app servers. As a part of this process we want to add some of the Apache response headers for security purpose. We are testing the settings one by one on all app servers. As per details mentioned below enable these headers for Apache:



Install httpd package on App Server 1 using yum and configure it to run on 8081 port, make sure to start its service.

Create an index.html file under Apache's default document root i.e /var/www/html and add below given content in it.

Welcome to the xFusionCorp Industries!

Configure Apache to enable below mentioned headers:

X-XSS-Protection header with value 1; mode=block

X-Frame-Options header with value SAMEORIGIN

X-Content-Type-Options header with value nosniff

Note: You can test using curl on the given app server as LBR URL will not work for this task.





To enable the desired headers on Apache web server, follow these steps:

1. Install the `httpd` package on App Server 1 using yum:
```
sudo yum install httpd
```

2. Configure Apache to run on port 8081:
Edit the Apache configuration file located at `/etc/httpd/conf/httpd.conf` and modify the `Listen` directive to listen on port 8081:
```
sudo vi /etc/httpd/conf/httpd.conf
```
Update the line `Listen 80` to `Listen 8081`, then save and exit the file.

3. Start the Apache service:
```
sudo systemctl start httpd
```

4. Create the index.html file with the given content:
```
sudo echo "Welcome to the xFusionCorp Industries!" > /var/www/html/index.html
OR
sudo sh -c 'echo "Welcome to the xFusionCorp Industries!" > /var/www/html/index.html'
```

5. Configure Apache to enable the desired headers:
Edit the Apache configuration file `/etc/httpd/conf/httpd.conf` and add the following lines at the end of the file:
```
Header set X-XSS-Protection "1; mode=block"
Header always append X-Frame-Options SAMEORIGIN
Header set X-Content-Type-Options nosniff
```
Save and exit the file.

6. Restart the Apache service to apply the changes:
```
sudo systemctl restart httpd
```

Now, Apache web server on App Server 1 is configured with the specified response headers. You can test the headers using curl or any other tool by accessing the server's IP address with port 8081.

TASK 48 - ADD RESPONSE HEADERS IN APACHE

We are working on hardening Apache web server on all app servers. As a part of this process we want to add some of the Apache response headers for security purpose. We are testing the settings one by one on all app servers. As per details mentioned below enable these headers for Apache:



Install httpd package on App Server 1 using yum and configure it to run on 8081 port, make sure to start its service.

Create an index.html file under Apache's default document root i.e /var/www/html and add below given content in it.

Welcome to the xFusionCorp Industries!

Configure Apache to enable below mentioned headers:

X-XSS-Protection header with value 1; mode=block

X-Frame-Options header with value SAMEORIGIN

X-Content-Type-Options header with value nosniff

Note: You can test using curl on the given app server as LBR URL will not work for this task.





To enable the desired headers on Apache web server, follow these steps:

1. Install the `httpd` package on App Server 1 using yum:
```
sudo yum install httpd
```

2. Configure Apache to run on port 8081:
Edit the Apache configuration file located at `/etc/httpd/conf/httpd.conf` and modify the `Listen` directive to listen on port 8081:
```
sudo vi /etc/httpd/conf/httpd.conf
```
Update the line `Listen 80` to `Listen 8081`, then save and exit the file.

3. Start the Apache service:
```
sudo systemctl start httpd
```

4. Create the index.html file with the given content:
```
sudo echo "Welcome to the xFusionCorp Industries!" > /var/www/html/index.html
OR
sudo sh -c 'echo "Welcome to the xFusionCorp Industries!" > /var/www/html/index.html'
```

5. Configure Apache to enable the desired headers:
Edit the Apache configuration file `/etc/httpd/conf/httpd.conf` and add the following lines at the end of the file:
```
Header set X-XSS-Protection "1; mode=block"
Header always append X-Frame-Options SAMEORIGIN
Header set X-Content-Type-Options nosniff
```
Save and exit the file.

6. Restart the Apache service to apply the changes:
```
sudo systemctl restart httpd
```
7. Test the server 

```
curl -I http://localhost:8081
```

Now, Apache web server on App Server 1 is configured with the specified response headers. You can test the headers using curl or any other tool by accessing the server's IP address with port 8081.

