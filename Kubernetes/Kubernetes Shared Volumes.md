We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.



Create a pod named volume-share-devops.


For the first container, use image fedora with latest tag only and remember to mention the tag i.e fedora:latest, container should be named as volume-container-devops-1, and run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/blog.


For the second container, use image fedora with the latest tag only and remember to mention the tag i.e fedora:latest, container should be named as volume-container-devops-2, and again run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/games.


Volume name should be volume-share of type emptyDir.


After creating the pod, exec into the first container i.e volume-container-devops-1, and just for testing create a file blog.txt with any content under the mounted path of first container i.e /tmp/blog.


The file blog.txt should be present under the mounted path /tmp/games on the second container volume-container-devops-2 as well, since they are using a shared volume.


----------------------


To achieve this, you can create a Kubernetes YAML file with the required pod configuration. Below is an example of how you can define the pod configuration:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-devops
spec:
  containers:
    - name: volume-container-devops-1
      image: fedora:latest
      command: ["sleep", "infinity"]  # Keeps the container running
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/blog
    - name: volume-container-devops-2
      image: fedora:latest
      command: ["sleep", "infinity"]  # Keeps the container running
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/games
  volumes:
    - name: volume-share
      emptyDir: {}
```

Save this YAML configuration in a file (e.g., `volume-share-pod.yaml`) and then apply it to your Kubernetes cluster using the `kubectl` command:

```bash
kubectl apply -f volume-share-pod.yaml
```

This will create the pod with two containers (`volume-container-devops-1` and `volume-container-devops-2`) sharing an emptyDir volume named `volume-share`. After applying the configuration, you can execute into the first container and create a file in the `/tmp/blog` directory. This file should also be present in the `/tmp/games` directory of the second container due to the shared volume.

To exec into the first container and create the file:

```bash
kubectl exec -it volume-share-devops -c volume-container-devops-1 -- /bin/sh
echo "This is a test file" > /tmp/blog/blog.txt
exit
```

The `blog.txt` file should now be present in both `/tmp/blog` and `/tmp/games` directories within the pods.


Verify the files in the share volume
```bash
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- cat /tmp/blog/blog.txt
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- cat /tmp/games/blog.txt
```

Please note that this example assumes that you have the necessary permissions to create pods in the Kubernetes cluster and use the `kubectl` command-line tool. Adjust the configuration or file paths as needed to match your specific requirements.