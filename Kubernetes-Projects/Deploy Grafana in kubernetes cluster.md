The Nautilus DevOps teams is planning to set up a Grafana tool to collect and analyze analytics from some applications. They are planning to deploy it on Kubernetes cluster. Below you can find more details.



1.) Create a deployment named grafana-deployment-xfusion using any grafana image for Grafana app. Set other parameters as per your choice.


2.) Create NodePort type service with nodePort 32000 to expose the app.


You need not to make any configuration changes inside the Grafana app once deployed, just make sure you are able to access the Grafana login page.


-----------------

To set up Grafana on a Kubernetes cluster, you can create a deployment and a NodePort service using the following YAML configurations:

1. Create a file named `grafana-deployment.yaml` and add the following content:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana-container
        image: <your-grafana-image>:<image-tag>
        ports:
        - containerPort: 3000
```

Replace `<your-grafana-image>` and `<image-tag>` with the Grafana image you want to use and its corresponding tag.

2. Create a file named `grafana-service.yaml` and add the following content:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: grafana-service
spec:
  type: NodePort
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 32000
  selector:
    app: grafana
```

Now, you can apply these configurations to your Kubernetes cluster:

```bash
kubectl apply -f grafana-deployment.yaml
kubectl apply -f grafana-service.yaml
```

Make sure Grafana's login page is accessible using the NodePort you've specified (in this case, `http://<node-ip>:32000`). You may need to get the external IP of one of your cluster nodes to access it, depending on your Kubernetes setup.