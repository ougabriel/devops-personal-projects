The Nautilus development team has completed development of one of the node applications, which they are planning to deploy on a Kubernetes cluster. They recently had a meeting with the DevOps team to share their requirements. Based on that, the DevOps team has listed out the exact requirements to deploy the app. Find below more details:


Create a deployment using gcr.io/kodekloud/centos-ssh-enabled:node image, replica count must be 2.

Create a service to expose this app, the service type must be NodePort, targetPort must be 8080 and nodePort should be 30012.

Make sure all the pods are in Running state after the deployment.

You can check the application by clicking on NodeApp button on top bar.


You can use any labels as per your choice.


-------------------


To deploy the Node.js application on a Kubernetes cluster with the specified requirements, you can create a Deployment and a NodePort Service. You can use the following YAML configuration files to achieve this:

1. Create a Deployment YAML file (e.g., `node-app-deployment.yaml`):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
        - name: node-app-container
          image: gcr.io/kodekloud/centos-ssh-enabled:node
          ports:
            - containerPort: 8080
```

2. Create a Service YAML file (e.g., `node-app-service.yaml`):

```yaml
apiVersion: v1
kind: Service
metadata:
  name: node-app-service
spec:
  type: NodePort
  selector:
    app: node-app
  ports:
    - protocol: TCP
      targetPort: 8080
      port: 8080
      nodePort: 30012
```

Now, apply these configuration files to your Kubernetes cluster:

```bash
kubectl apply -f node-app-deployment.yaml
kubectl apply -f node-app-service.yaml
```

This will create a Deployment with two replicas of your Node.js application and a NodePort Service to expose it. The service will listen on port 8080 inside the cluster and expose it on port 30012 on the nodes. The pods will be labeled with `app: node-app` to match the selector in the Service configuration.

Ensure that all the pods are running:

```bash
kubectl get pods
```

You should see two pods in the "Running" state.

Now, you can access your application by using the NodePort. Click on the "NodeApp" button and access the application using the node's IP address and the NodePort (e.g., http://<node-ip>:30012).



