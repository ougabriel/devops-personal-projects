The Nautilus DevOps team want to deploy a PHP website on Kubernetes cluster. They are going to use Apache as a web server and Mysql for database. The team had already gathered the requirements and now they want to make this website live. Below you can find more details:



1) Create a config map php-config for php.ini with variables_order = "EGPCS" data.


2) Create a deployment named lamp-wp.


3) Create two containers under it. First container must be httpd-php-container using image webdevops/php-apache:alpine-3-php7 and second container must be mysql-container from image mysql:5.6. Mount php-config configmap in httpd container at /opt/docker/etc/php/php.ini location.


4) Create kubernetes generic secrets for mysql related values like myql root password, mysql user, mysql password, mysql host and mysql database. Set any values of your choice.


5) Add some environment variables for both containers:


a) MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER, MYSQL_PASSWORD and MYSQL_HOST. Take their values from the secrets you created. Please make sure to use env field (do not use envFrom) to define the name-value pair of environment variables.


6) Create a node port type service lamp-service to expose the web application, nodePort must be 30008.


7) Create a service for mysql named mysql-service and its port must be 3306.


8) We already have /tmp/index.php file on jump_host server.


a) Copy this file into httpd container under Apache document root i.e /app and replace the dummy values for mysql related variables with the environment variables you have set for mysql related parameters. Please make sure you do not hard code the mysql related details in this file, you must use the environment variables to fetch those values.


b) You must be able to access this index.php on node port 30008 at the end, please note that you should see Connected successfully message while accessing this page.



----


Certainly, I've arranged the steps in a clear and concise format:

### Step 1: Create ConfigMap for php.ini
```bash
kubectl create configmap php-config --from-literal=php.ini="variables_order = 'EGPCS'"
```

### Step 2: Create Deployment
Create a YAML file named `lamp-wp.yaml` with the following content:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: lamp-wp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: lamp-wp
  template:
    metadata:
      labels:
        app: lamp-wp
    spec:
      containers:
        - name: httpd-php-container
          image: webdevops/php-apache:alpine-3-php7
          ports:
            - containerPort: 80
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: database
                  key: MYSQL_ROOT_PASSWORD
            - name: MYSQL_DATABASE
              valueFrom:
                secretKeyRef:
                  name: database
                  key: MYSQL_DATABASE
            - name: MYSQL_USER
              valueFrom:
                secretKeyRef:
                  name: database
                  key: MYSQL_USER
            - name: MYSQL_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: database
                  key: MYSQL_PASSWORD
            - name: MYSQL_HOST
              valueFrom:
                secretKeyRef:
                  name: database
                  key: MYSQL_HOST
          volumeMounts:
            - name: php-ini
              mountPath: /opt/docker/etc/php/php.ini
              subPath: php.ini
      
        - name: mysql-container
          image: mysql:5.6
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: database
                  key: MYSQL_ROOT_PASSWORD
            - name: MYSQL_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: database
                  key: MYSQL_PASSWORD
            - name: MYSQL_DATABASE
              valueFrom:
                secretKeyRef:
                  name: database
                  key: MYSQL_DATABASE
            - name: MYSQL_USER
              valueFrom:
                secretKeyRef:
                  name: database
                  key: MYSQL_USER
            - name: MYSQL_HOST
              valueFrom:
                secretKeyRef:
                  name: database
                  key: MYSQL_HOST
          ports:
            - containerPort: 3306
        
      volumes:
        - name: php-ini
          configMap:
            name: php-config
```

Apply the configuration:

```bash
kubectl apply -f lamp-wp.yaml
```

### Step 3: Create MySQL Secrets
```bash
kubectl create secret generic database \
--from-literal=MYSQL_ROOT_PASSWORD=admin123 \
--from-literal=MYSQL_DATABASE=kodekloud \
--from-literal=MYSQL_USER=kodekloud \
--from-literal=MYSQL_PASSWORD=admin123 \
--from-literal=MYSQL_HOST=mysql-service
```

### Step 4: Create Services
Create services for the web application and MySQL:

#### lamp-service.yaml
```yaml
apiVersion: v1
kind: Service
metadata:
  name: lamp-service
spec:
  type: NodePort
  selector:
    app: lamp-wp
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008
```

#### mysql-service.yaml
```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql-service
spec:
  selector:
    app: lamp-wp
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
```

Apply the service configurations:

```bash
kubectl apply -f lamp-service.yaml
kubectl apply -f mysql-service.yaml
```

### Step 5: Update index.php
Edit the `/tmp/index.php` file on your local machine. Replace it with the following code, using the environment variables for database credentials:

```php
<?php
$dbname = $_ENV["MYSQL_DATABASE"];
$dbuser = $_ENV["MYSQL_USER"];
$dbpass = $_ENV["MYSQL_PASSWORD"];
$dbhost = $_ENV["MYSQL_HOST"];

$connect = mysqli_connect($dbhost, $dbuser, $dbpass) or die("Unable to Connect to '$dbhost'");

$test_query = "SHOW TABLES FROM $dbname";
$result = mysqli_query($test_query);

if ($result->connect_error) {
   die("Connection failed: " . $conn->connect_error);
}
echo "Connected successfully";
```

### Step 6: Copy index.php to Container
Copy the updated `index.php` file into the HTTP container:

```bash
kubectl cp /tmp/index.php $POD:/app/index.php -c $HTTP
```bash


t