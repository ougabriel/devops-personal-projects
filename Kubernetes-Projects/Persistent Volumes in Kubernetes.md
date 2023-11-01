The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:


Create a PersistentVolume named as pv-nautilus. Configure the spec as storage class should be manual, set capacity to 4Gi, set access mode to ReadWriteOnce, volume type should be hostPath and set path to /mnt/sysops (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

Create a PersistentVolumeClaim named as pvc-nautilus. Configure the spec as storage class should be manual, request 3Gi of the storage, set access mode to ReadWriteOnce.

Create a pod named as pod-nautilus, mount the persistent volume you created with claim name pvc-nautilus at document root of the web server, the container within the pod should be named as container-nautilus using image httpd with latest tag only (remember to mention the tag i.e httpd:latest).

Create a node port type service named web-nautilus using node port 30008 to expose the web server running within the pod.



-------------


To achieve the requirements mentioned, you can create the necessary PersistentVolume (PV), PersistentVolumeClaim (PVC), Pod, and Service in a Kubernetes YAML configuration file. Here's a step-by-step guide and the corresponding YAML definitions for each component:

1. **Create a PersistentVolume (pv-nautilus):**

   Create a YAML file, for example, `pv-nautilus.yaml`, with the following content:

   ```yaml
   apiVersion: v1
   kind: PersistentVolume
   metadata:
     name: pv-nautilus
   spec:
     storageClassName: manual
     capacity:
       storage: 4Gi
     accessModes:
       - ReadWriteOnce
     hostPath:
       path: "/mnt/sysops"
   ```

   Apply the configuration using `kubectl apply -f pv-nautilus.yaml`.

2. **Create a PersistentVolumeClaim (pvc-nautilus):**

   Create a YAML file, for example, `pvc-nautilus.yaml`, with the following content:

   ```yaml
   apiVersion: v1
   kind: PersistentVolumeClaim
   metadata:
     name: pvc-nautilus
   spec:
     storageClassName: manual
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 3Gi
   ```

   Apply the configuration using `kubectl apply -f pvc-nautilus.yaml`.

3. **Create a Pod (pod-nautilus):**

   Create a YAML file, for example, `pod-nautilus.yaml`, with the following content:

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: pod-nautilus
   spec:
     containers:
       - name: container-nautilus
         image: httpd:latest
         ports:
           - containerPort: 80
         volumeMounts:
           - name: web-content
             mountPath: /usr/local/apache2/htdocs
     volumes:
       - name: web-content
         persistentVolumeClaim:
           claimName: pvc-nautilus
   ```

   Apply the configuration using `kubectl apply -f pod-nautilus.yaml`.

4. **Create a NodePort Service (web-nautilus):**

   Create a YAML file, for example, `service-nautilus.yaml`, with the following content:

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: web-nautilus
   spec:
     selector:
       name: pod-nautilus
     ports:
       - protocol: TCP
         port: 80
         targetPort: 80
       - nodePort: 30008
         port: 80
         protocol: TCP
     type: NodePort
   ```

   Apply the configuration using `kubectl apply -f service-nautilus.yaml`.

