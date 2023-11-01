There are a number of parameters that are used by the applications. We need to define these as environment variables, so that we can use them as needed within different configs. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about the same.


Create a pod named envars.

Container name should be fieldref-container, use image nginx preferable latest tag, use command 'sh', '-c' and args should be

'while true; do echo -en '/n'; printenv NODE_NAME POD_NAME; printenv POD_IP POD_SERVICE_ACCOUNT; sleep 10; done;'

(Note: please take care of indentations)

Define Four environment variables as mentioned below:
a.) The first env should be named as NODE_NAME, set valueFrom fieldref and fieldPath should be spec.nodeName.

b.) The second env should be named as POD_NAME, set valueFrom fieldref and fieldPath should be metadata.name.

c.) The third env should be named as POD_IP, set valueFrom fieldref and fieldPath should be status.podIP.

d.) The fourth env should be named as POD_SERVICE_ACCOUNT, set valueFrom fieldref and fieldPath shoulbe be spec.serviceAccountName.

Set restart policy to Never.

To check the output, exec into the pod and use printenv comma


--------------------------

You can create the Kubernetes pod with the specified environment variables and configuration using the following YAML manifest:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: envars
spec:
  containers:
    - name: fieldref-container
      image: nginx:latest
      command: ["sh", "-c"]
      args:
        - "while true; do echo -e '\n'; printenv NODE_NAME POD_NAME; printenv POD_IP POD_SERVICE_ACCOUNT; sleep 10; done;"
      env:
        - name: NODE_NAME
          valueFrom:
            fieldRef:
              fieldPath: spec.nodeName
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        - name: POD_IP
          valueFrom:
            fieldRef:
              fieldPath: status.podIP
        - name: POD_SERVICE_ACCOUNT
          valueFrom:
            fieldRef:
              fieldPath: spec.serviceAccountName
  restartPolicy: Never
```

Save this YAML to a file (e.g., `envars-pod.yaml`) and apply it to your Kubernetes cluster using the `kubectl apply` command:

```bash
kubectl apply -f envars-pod.yaml
```

This will create a pod named `envars` with the specified environment variables and configuration. To check the output, you can exec into the pod and run the `printenv` command:

```bash
kubectl exec -it envars -- printenv
```

This will display the values of the defined environment variables within the pod.