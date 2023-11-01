INSTALL JENKINS ON KUBERNETES

The Nautilus DevOps team is planning to set up a Jenkins CI server to create/manage some deployment pipelines for some of the projects. They want to set up the Jenkins server on Kubernetes cluster. Below you can find more details about the task:



1) Create a namespace jenkins

2) Create a Service for jenkins deployment. Service name should be jenkins-service under jenkins namespace, type should be NodePort, nodePort should be 30008

3) Create a Jenkins Deployment under jenkins namespace, It should be name as jenkins-deployment , labels app should be jenkins , container name should be jenkins-container , use jenkins/jenkins image , containerPort should be 8080 and replicas count should be 1.

Make sure to wait for the pods to be in running state and make sure you are able to access the Jenkins login screen in the browser before hitting the Check button.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.



To set up the Jenkins CI server on a Kubernetes cluster, you can follow the steps provided below. Make sure to run the commands on the jump_host, where the kubectl utility is configured to work with the Kubernetes cluster.

1. Create the `jenkins` namespace:

```bash
kubectl create namespace jenkins
```

2. Create a Service for the Jenkins deployment:

```bash
kubectl apply -n jenkins -f - <<EOF
apiVersion: v1
kind: Service
metadata:
  name: jenkins-service
spec:
  type: NodePort
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30008
  selector:
    app: jenkins
EOF
```

3. Create the Jenkins Deployment:

```bash
kubectl apply -n jenkins -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins
  template:
    metadata:
      labels:
        app: jenkins
    spec:
      containers:
        - name: jenkins-container
          image: jenkins/jenkins
          ports:
            - containerPort: 8080
EOF
```

4. Wait for the Jenkins pod to be in the running state:

```bash
kubectl wait --for=condition=Ready pod -n jenkins -l app=jenkins --timeout=300s
```

5. Access the Jenkins login screen in the browser:

Open a web browser and navigate to the Jenkins URL using the NodePort you specified earlier. For example, if your Kubernetes cluster's IP address is `192.168.0.100`, access Jenkins using:

```
http://192.168.0.100:30008
```

Note: The first time you access Jenkins, you may need to retrieve the initial admin password from the Jenkins container logs and enter it during the setup.

After completing these steps, you should have a Jenkins CI server set up on your Kubernetes cluster, and you should be able to access the Jenkins login screen in the browser. Once you have confirmed successful access, you can click the Check button.