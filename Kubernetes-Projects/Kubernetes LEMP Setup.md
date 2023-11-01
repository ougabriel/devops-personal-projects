The Nautilus DevOps team want to deploy a static website on Kubernetes cluster. They are going to use Nginx, phpfpm and MySQL for the database. The team had already gathered the requirements and now they want to make this website live. Below you can find more details:

Create some secrets for MySQL.

Create a secret named mysql-root-pass wih key/value pairs as below:

name: password
value: R00t  
Create a secret named mysql-user-pass with key/value pairs as below:

name: username
value: kodekloud_aim

name: password
value: LQfKeWWxWD  
Create a secret named mysql-db-url with key/value pairs as below:

name: database
value: kodekloud_db9  
Create a secret named mysql-host with key/value pairs as below:

name: host
value: mysql-service  
Create a config map php-config for php.ini with variables_order = "EGPCS" data.

Create a deployment named lemp-wp.

Create two containers under it:

-First container must be nginx-php-container using image webdevops/php-nginx:alpine-3-php7 and

second container must be mysql-container from image mysql:5.6.
Mount php-config configmap in nginx container at /opt/docker/etc/php/php.ini location.

Add some environment variables for both containers. Take their values from the secrets you created.

MYSQL_ROOT_PASSWORD
MYSQL_DATABASE
MYSQL_USER
MYSQL_PASSWORD
MYSQL_HOST
Create a node port type service lemp-service to expose the web application, nodePort must be 30008.

Create a service for mysql named mysql-service and its port must be 3306.

We already have a /tmp/index.php file on jump_host server.

Copy this file into the nginx container under document root i.e /app and replace the dummy values for mysql related variables with the environment variables you have set for mysql related parameters. Please make sure you do not hard code the mysql related details in this file, you must use the environment variables to fetch those values.

Once done, you must be able to access this website using Website button on the top bar, please note that you should see Connected successfully message while accessing this page.


----------

To achieve this task and deploy the static website on a Kubernetes cluster, you can follow these steps. Please note that you should have `kubectl` access to your Kubernetes cluster.

1. **Create Secrets for MySQL:**

   Create the following secrets to store MySQL credentials and configuration:

   ```yaml
   apiVersion: v1
   kind: Secret
   metadata:
     name: mysql-root-pass
   data:
     password: R00t

   apiVersion: v1
   kind: Secret
   metadata:
     name: mysql-user-pass
   data:
     username: kodekloud_aim
     password: LQfKeWWxWD

   apiVersion: v1
   kind: Secret
   metadata:
     name: mysql-db-url
   data:
     database: kodekloud_db9

   apiVersion: v1
   kind: Secret
   metadata:
     name: mysql-host
   data:
     host: mysql-service
   ```

   Apply these secrets to your cluster using `kubectl apply -f secret.yaml`.

2. **Create a ConfigMap for php-config:**

   Create a ConfigMap to store PHP configuration:

   ```yaml
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: php-config
   data:
     php.ini: |
       variables_order = "EGPCS"
   ```

   Apply the ConfigMap using `kubectl apply -f configmap.yaml`.

3. **Create a Deployment for lemp-wp:**

   Create a Deployment that includes two containers, one for Nginx and PHP (nginx-php-container) and the other for MySQL (mysql-container). Mount the `php-config` ConfigMap to the Nginx container.

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: lemp-wp
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: lemp-wp
     template:
       metadata:
         labels:
           app: lemp-wp
       spec:
         containers:
           - name: nginx-php-container
             image: webdevops/php-nginx:alpine-3-php7
             volumeMounts:
               - name: php-config-volume
                 mountPath: /opt/docker/etc/php/php.ini
                 subPath: php.ini
             env:
               - name: MYSQL_ROOT_PASSWORD
                 valueFrom:
                   secretKeyRef:
                     name: mysql-root-pass
                     key: password
               - name: MYSQL_DATABASE
                 valueFrom:
                   secretKeyRef:
                     name: mysql-db-url
                     key: database
               - name: MYSQL_USER
                 valueFrom:
                   secretKeyRef:
                     name: mysql-user-pass
                     key: username
               - name: MYSQL_PASSWORD
                 valueFrom:
                   secretKeyRef:
                     name: mysql-user-pass
                     key: password
               - name: MYSQL_HOST
                 valueFrom:
                   secretKeyRef:
                     name: mysql-host
                     key: host
           - name: mysql-container
             image: mysql:5.6
             env:
               - name: MYSQL_ROOT_PASSWORD
                 valueFrom:
                   secretKeyRef:
                     name: mysql-root-pass
                     key: password
               - name: MYSQL_DATABASE
                 valueFrom:
                   secretKeyRef:
                     name: mysql-db-url
                     key: database
               - name: MYSQL_USER
                 valueFrom:
                   secretKeyRef:
                     name: mysql-user-pass
                     key: username
               - name: MYSQL_PASSWORD
                 valueFrom:
                   secretKeyRef:
                     name: mysql-user-pass
                     key: password
   ```

   Apply this Deployment using `kubectl apply -f deployment.yaml`.

4. **Create a NodePort Service for lemp-service:**

   Create a NodePort service to expose the web application on a specific port (e.g., 30008):

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: lemp-service
   spec:
     type: NodePort
     selector:
       app: lemp-wp
     ports:
       - protocol: TCP
         port: 80
         targetPort: 80
         nodePort: 30008
   ```

   Apply this service using `kubectl apply -f service.yaml`.

5. **Create a Service for MySQL:**

   Create a service to allow communication with the MySQL container:

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: mysql-service
   spec:
     ports:
       - protocol: TCP
         port: 3306
     selector:
       app: lemp-wp
   ```

   Apply this service using `kubectl apply -f mysql-service.yaml`.

6. **Copy `/tmp/index.php` to Nginx Container:**

   Use the `kubectl cp` command to copy the `/tmp/index.php` file from your Jump Host into the Nginx container under the document root `/app`. Ensure you replace the dummy values in the `index.php` file with the appropriate environment variables you set in the deployment.

   ```bash
   kubectl cp /tmp/index.php lemp-wp-<pod-id>:/app/index.php
   ```

7. **Access the Website:**

   Once the deployment and services are created, you should be able to access the website using the NodePort you specified (e.g., `http://<node-ip>:30008`). You should see the "Connected successfully" message.

Make sure to replace `<pod-id>` and `<node-ip>` with the actual values from your cluster.