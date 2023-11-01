DEPLOY AN APACHE WEBSERVER ON A KUBERNETES CLUSTER

There is an application that needs to be deployed on Kubernetes cluster under Apache web server. The Nautilus application development team has asked the DevOps team to deploy it. We need to develop a template as per requirements mentioned below:



Create a namespace named as httpd-namespace-devops.

Create a deployment named as httpd-deployment-devops under newly created namespace. For the deployment use httpd image with latest tag only and remember to mention the tag i.e httpd:latest, and make sure replica counts are 2.

Create a service named as httpd-service-devops under same namespace to expose the deployment, nodePort should be 30004.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.




To create the necessary resources for deploying the application on the Kubernetes cluster, you can follow these steps:

1. Create the namespace:
   ```
   kubectl create namespace httpd-namespace-devops
   ```

2. Create the deployment:
   Save the following YAML code in a file named `httpd-deployment.yaml`:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: httpd-deployment-devops
     namespace: httpd-namespace-devops
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: httpd
     template:
       metadata:
         labels:
           app: httpd
       spec:
         containers:
           - name: httpd-container
             image: httpd:latest
             ports:
               - containerPort: 80
   ```

   Apply the configuration:
   ```
   kubectl apply -f httpd-deployment.yaml
   ```

3. Create the service:
   Save the following YAML code in a file named `httpd-service.yaml`:

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: httpd-service-devops
     namespace: httpd-namespace-devops
   spec:
     type: NodePort
     selector:
       app: httpd
     ports:
       - protocol: TCP
         port: 80
         targetPort: 80
         nodePort: 30004
   ```

   Apply the configuration:
   ```
   kubectl apply -f httpd-service.yaml
   ```

After executing these commands, the application will be deployed on the Kubernetes cluster under the Apache web server. The deployment will have two replicas, and the service will be exposed on port 30004.