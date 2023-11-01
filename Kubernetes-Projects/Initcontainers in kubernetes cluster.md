There are some applications that need to be deployed on Kubernetes cluster and these apps have some pre-requisites where some configurations need to be changed before deploying the app container. Some of these changes cannot be made inside the images so the DevOps team has come up with a solution to use init containers to perform these tasks during deployment. Below is a sample scenario that the team is going to test first.



Create a Deployment named as ic-deploy-devops.


Configure spec as replicas should be 1, labels app should be ic-devops, template's metadata lables app should be the same ic-devops.


The initContainers should be named as ic-msg-devops, use image debian, preferably with latest tag and use command '/bin/bash', '-c' and 'echo Init Done - Welcome to xFusionCorp Industries > /ic/news'. The volume mount should be named as ic-volume-devops and mount path should be /ic.


Main container should be named as ic-main-devops, use image debian, preferably with latest tag and use command '/bin/bash', '-c' and 'while true; do cat /ic/news; sleep 5; done'. The volume mount should be named as ic-volume-devops and mount path should be /ic.


Volume to be named as ic-volume-devops and it should be an emptyDir type.




-------------------


You can create a Kubernetes Deployment named `ic-deploy-devops` with init containers to perform specific tasks before the main container starts. Here's a YAML configuration that accomplishes this:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-devops
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-devops
  template:
    metadata:
      labels:
        app: ic-devops
    spec:
      initContainers:
        - name: ic-msg-devops
          image: debian:latest
          command: ["/bin/bash", "-c", "echo 'Init Done - Welcome to xFusionCorp Industries' > /ic/news"]
          volumeMounts:
            - name: ic-volume-devops
              mountPath: /ic
      containers:
        - name: ic-main-devops
          image: debian:latest
          command: ["/bin/bash", "-c", "while true; do cat /ic/news; sleep 5; done"]
          volumeMounts:
            - name: ic-volume-devops
              mountPath: /ic
      volumes:
        - name: ic-volume-devops
          emptyDir: {}
```

You can apply this configuration using `kubectl apply -f ic-deploy-devops.yaml`. This will create a Deployment with one replica and two containers. The `ic-msg-devops` init container will echo a welcome message into the `/ic/news` file, and the `ic-main-devops` container will continuously read and display the content of that file. The volume `ic-volume-devops` is used to share data between the init container and the main container.

