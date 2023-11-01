The Nautilus Application Development team is planning to deploy one of the php-based applications on Kubernetes cluster. As per the recent discussion with DevOps team, they have decided to use nginx and phpfpm. Additionally, they also shared some custom configuration requirements. Below you can find more details. Please complete this task as per requirements mentioned below:



1) Create a service to expose this app, the service type must be NodePort, nodePort should be 30012.


2.) Create a config map named nginx-config for nginx.conf as we want to add some custom settings in nginx.conf.


a) Change the default port 80 to 8099 in nginx.conf.


b) Change the default document root /usr/share/nginx to /var/www/html in nginx.conf.


c) Update the directory index to index index.html index.htm index.php in nginx.conf.


3.) Create a pod named nginx-phpfpm .


b) Create a shared volume named shared-files that will be used by both containers (nginx and phpfpm) also it should be a emptyDir volume.


c) Map the ConfigMap we declared above as a volume for nginx container. Name the volume as nginx-config-volume, mount path should be /etc/nginx/nginx.conf and subPath should be nginx.conf


d) Nginx container should be named as nginx-container and it should use nginx:latest image. PhpFPM container should be named as php-fpm-container and it should use php:7.4-fpm-alpine image.


e) The shared volume shared-files should be mounted at /var/www/html location in both containers. Copy /opt/index.php from jump host to the nginx document root inside the nginx container, once done you can access the app using App button on the top bar.


Before clicking on finish button always make sure to check if all pods are in running state.


You can use any labels as per your choice.

-------


To deploy the PHP-based application on a Kubernetes cluster with Nginx and PHP-FPM, along with the custom configurations, you can create the necessary Kubernetes manifests as follows:

### 1. Create a NodePort Service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: php-app-service
spec:
  selector:
    app: nginx-phpfpm
  ports:
    - protocol: TCP
      port: 8099  # Change port to 8099 as per requirement
      targetPort: 80
  type: NodePort
  nodePort: 30012  # Set nodePort to 30012 as per requirement
```

### 2. Create a ConfigMap for nginx.conf:

```yaml
kind: ConfigMap
metadata:
  name: nginx-config
data:
  nginx.conf: |
    events {} 
    http {
      server {
        listen 8099;
        index index.html index.htm index.php;
        root  /var/www/html;
        location ~ \.php$ {
          include fastcgi_params;
          fastcgi_param REQUEST_METHOD $request_method;
          fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
          fastcgi_pass 127.0.0.1:9000;
        }
     
```

### 3. Create a Pod with Nginx and PHP-FPM Containers:

```yaml
---       
apiVersion: v1
kind: Pod
metadata:
  name: nginx-phpfpm
  labels:
     app: nginx-phpfpm
     tier: back-end
spec:
  volumes:
    - name: shared-files
      emptyDir: {}
    - name: nginx-config-volume
      configMap:
        name: nginx-config
  containers:
    - name: nginx-container
      image: nginx:latest
      volumeMounts:
        - name: shared-files
          mountPath: /var/www/html
        - name: nginx-config-volume
          mountPath: /etc/nginx/nginx.conf
          subPath: nginx.conf
    - name: php-fpm-container
      image: php:8.2-fpm-alpine
      volumeMounts:
        - name: shared-files
          mountPath: /var/www/html  
```
 Next, check the index.php file first before copying it onto the nginx-container inside the pod we created.

```bash
cat /opt/index.php  
```
```bash
kubectl exec -it nginx-phpfpm -c nginx-container -- cp /opt/index.php /var/www/html/ 
```
 or
 
```bash
 kubectl cp /opt/index.php nginx-phpfpm:/var/www/html/ -c nginx-container 
```
Check if the file is indeed copied onto the container.

```bash
kubectl exec -it nginx-phpfpm -c nginx-container -- cat /var/www/html/index.php
```
Finally, verify by running the curl command inside the container. We can open shell on the container and run the curl inside it or we can simply run the curl directly using the exec command.

```bash
kubectl exec -it nginx-phpfpm -c nginx-container -- curl http://localhost:8099
```