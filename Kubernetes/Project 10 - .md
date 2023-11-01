
The Nautilus DevOps team want to create a time check pod in a particular Kubernetes namespace and record the logs. This might be initially used only for testing purposes, but later can be implemented in an existing cluster. Please find more details below about the task and perform it.


Create a pod called time-check in the nautilus namespace. This pod should run a container called time-check, container should use the busybox image with latest tag only and remember to mention tag i.e busybox:latest.

Create a config map called time-config with the data TIME_FREQ=4 in the same namespace.

The time-check container should run the command: while true; do date; sleep $TIME_FREQ;done and should write the result to the location /opt/finance/time/time-check.log. Remember you will also need to add an environmental variable TIME_FREQ in the container, which should pick value from the config map TIME_FREQ key.

Create a volume log-volume and mount the same on /opt/finance/time within the container.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

--------------------



Step 1: Create the config map named `time-config` in the `nautilus` namespace:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-config
  namespace: nautilus
data:
  TIME_FREQ: "4"
```

Save the above YAML content into a file, for example, `time-config.yaml`.

To create the config map, use the following `kubectl` command:

```bash
kubectl create -f time-config.yaml
```

Step 2: Create the pod named `time-check` in the `nautilus` namespace:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: time-check
  namespace: nautilus
spec:
  containers:
  - name: time-check
    image: busybox:latest
    command: ["/bin/sh", "-c", "while true; do date; sleep $TIME_FREQ; done > /opt/finance/time/time-check.log"]
    env:
    - name: TIME_FREQ
      valueFrom:
        configMapKeyRef:
          name: time-config
          key: TIME_FREQ
    volumeMounts:
    - name: log-volume
      mountPath: /opt/finance/time
  volumes:
  - name: log-volume
    emptyDir: {}
```

Save the above YAML content into a file, for example, `time-check-pod.yaml`.

To create the pod, use the following `kubectl` command:

```bash
kubectl create -f time-check-pod.yaml
```

With these steps, you have created a pod named `time-check` in the `nautilus` namespace. This pod runs a container called `time-check`, which uses the busybox:latest image. The container runs the specified command with the value of the `TIME_FREQ` environment variable picked from the `time-config` config map. The output of the command is redirected to the file `/opt/finance/time/time-check.log`, which is mounted to the `log-volume` volume. The `TIME_FREQ` is set to 4 as specified in the config map data.