A new java-based application is ready to be deployed on a Kubernetes cluster. The development team had a meeting with the DevOps team to share the requirements and application scope. The team is ready to setup an application stack for it under their existing cluster. Below you can find the details for this:


Create a namespace named tomcat-namespace-nautilus.

Create a deployment for tomcat app which should be named as tomcat-deployment-nautilus under the same namespace you created. Replica count should be 1, the container should be named as tomcat-container-nautilus, its image should be gcr.io/kodekloud/centos-ssh-enabled:tomcat and its container port should be 8080.

Create a service for tomcat app which should be named as tomcat-service-nautilus under the same namespace you created. Service type should be NodePort and nodePort should be 32227.


-------

Sure, you can create the required resources in Kubernetes as follows:

1. Create a namespace named `tomcat-namespace-nautilus` using a YAML file, let's call it `tomcat-namespace.yaml`:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: tomcat-namespace-nautilus
```

Apply this YAML to create the namespace:

```bash
kubectl apply -f tomcat-namespace.yaml
```

2. Create a deployment for the Tomcat app named `tomcat-deployment-nautilus` in the same namespace. Save this as `tomcat-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: tomcat-deployment-nautilus
  namespace: tomcat-namespace-nautilus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tomcat-app
  template:
    metadata:
      labels:
        app: tomcat-app
    spec:
      containers:
        - name: tomcat-container-nautilus
          image: gcr.io/kodekloud/centos-ssh-enabled:tomcat
          ports:
            - containerPort: 8080
```

Apply this YAML to create the deployment:

```bash
kubectl apply -f tomcat-deployment.yaml
```

3. Create a NodePort service for the Tomcat app named `tomcat-service-nautilus` in the same namespace. Save this as `tomcat-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: tomcat-service-nautilus
  namespace: tomcat-namespace-nautilus
spec:
  selector:
    app: tomcat-app
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
      nodePort: 32227
  type: NodePort

```

Apply this YAML to create the service:

```bash
kubectl apply -f tomcat-service.yaml
```

Now, you have a namespace named `tomcat-namespace-nautilus`, a deployment named `tomcat-deployment-nautilus`, and a NodePort service named `tomcat-service-nautilus` for your Tomcat application within that namespace.