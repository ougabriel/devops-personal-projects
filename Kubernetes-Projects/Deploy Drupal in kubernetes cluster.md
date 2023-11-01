We need to deploy a Drupal application on Kubernetes cluster. The Nautilus application development team want to setup a fresh Drupal as they will do the installation on their own. Below you can find the requirements, they have shared with us.



1) Configure a persistent volume drupal-mysql-pv with hostPath = /drupal-mysql-data (/drupal-mysql-data directory already exists on the worker Node i.e jump host), 5Gi of storage and ReadWriteOnce access mode.


2) Configure one PersistentVolumeClaim named drupal-mysql-pvc with storage request of 3Gi and ReadWriteOnce access mode.


3) Create a deployment drupal-mysql with 1 replica, use mysql:5.7 image. Mount the claimed PVC at /var/lib/mysql.


4) Create a deployment drupal with 1 replica and use drupal:8.6 image.


4) Create a NodePort type service which should be named as drupal-service and nodePort should be 30095.


5) Create a service drupal-mysql-service to expose mysql deployment on port 3306.


6) Set rest of the configration for deployments, services, secrets etc as per your preferences. At the end you should be able to access the Drupal installation page by clicking on App button.


-----

To deploy a Drupal application on a Kubernetes cluster with the given requirements, you can create Kubernetes manifest files for each resource using YAML and then apply those files to your cluster. Here are the manifest files for each of the requirements:

1. Configure a persistent volume named `drupal-mysql-pv`:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: drupal-mysql-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /drupal-mysql-data
```

2. Configure a PersistentVolumeClaim (PVC) named `drupal-mysql-pvc`:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: drupal-mysql-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```

3. Create a MySQL deployment (`drupal-mysql`) with a mounted PVC:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: drupal-mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: drupal-mysql
  template:
    metadata:
      labels:
        app: drupal-mysql
    spec:
      containers:
        - name: mysql
          image: mysql:5.7
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: your-root-password
          ports:
            - containerPort: 3306
          volumeMounts:
            - name: drupal-mysql-persistent-storage
              mountPath: /var/lib/mysql
      volumes:
        - name: drupal-mysql-persistent-storage
          persistentVolumeClaim:
            claimName: drupal-mysql-pvc
```

4. Create a Drupal deployment (`drupal`) using the Drupal image you prefer:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: drupal
spec:
  replicas: 1
  selector:
    matchLabels:
      app: drupal
  template:
    metadata:
      labels:
        app: drupal
    spec:
      containers:
        - name: drupal
          image: drupal:8.6
          ports:
            - containerPort: 80
```

5. Create a NodePort service for Drupal:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: drupal-service
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30095
  selector:
    app: drupal
```

6. Create a service to expose the MySQL deployment (`drupal-mysql-service`):

```yaml
apiVersion: v1
kind: Service
metadata:
  name: drupal-mysql-service
spec:
  ports:
    - port: 3306
  selector:
    app: drupal-mysql
```

7. You may need to create secrets for database credentials and any other configuration specific to your Drupal installation. Replace `your-root-password` with the actual password for the MySQL root user in the MySQL deployment.

Once you have these YAML files prepared, you can apply them to your Kubernetes cluster using the `kubectl apply -f filename.yaml` command for each file. After all resources are created, you should be able to access the Drupal installation page by using the NodePort service's IP and port 30095.