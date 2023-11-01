One of the Nautilus DevOps team members was working on to update an existing Kubernetes template. Somehow, he made some mistakes in the template and it is failing while applying. We need to fix this as soon as possible, so take a look into it and make sure you are able to apply it without any issues. Also, do not remove any component from the template like pods/deployments/volumes etc.

/home/thor/mysql_deployment.yml is the template that needs to be fixed.



-----------


Certainly, here's your GitHub post with the incorrect manifest file added:

**Note:**
The `kubectl` utility on the jump host has been pre-configured to work seamlessly with the Kubernetes cluster.

**Steps:**

1. The deployment is currently pending, and there are no logs available for inspection. The next step is to attempt the deployment and monitor for potential error messages.

   ```shell
   $ kubectl apply -f mysql_deployment.yml 
   ```

   This command may produce error messages such as:

   ```
   unable to recognize "mysql_deployment.yml": no matches for kind "PersistentVolume" in version "apps/v1"
   unable to recognize "mysql_deployment.yml": no matches for kind "PersistentVolumeClaim" in version "apps/v1"
   error validating "mysql_deployment.yml": error validating data: ValidationError(Service.spec): unknown field "tier" in io.k8s.api.core.v1.ServiceSpec; if you choose to ignore these errors, turn validation off with --validate=false  
   ```

   Now that an error message has been generated, we can proceed to analyze the deployment manifest.

   **Incorrect Manifest (mysql_deployment.yml):**

```yaml
apiVersion: apps/v1 
kind: PersistentVolume            
metadata:
  name: mysql-pv
  labels: 
  type: local 
spec:
  storageClassName: standard      
  capacity:
    storage: 250Mi
  accessModes: 
    - ReadWriteOnce
  hostPath:                       
  path: "/mnt/data" 
  persistentVolumeReclaimPolicy:  
  -  Retain  
---    
apiVersion: apps/v1 
kind: PersistentVolumeClaim 
metadata:                          
  name: mysql-pv-claim
  labels:
  app: mysql-app 
spec:                              
  storageClassName: standard       
  accessModes:
    - ReadWriteOnce                
  resources:
    requests: 
      storage: 250MB 
---
apiVersion: v1                    
kind: Service                      
metadata:
  name: mysql         
  labels:             
    app: mysql-app
spec:
  type: NodePort
  ports:
    - targetPort: 3306
      port: 3306
      nodePort: 30011
  selector:    
    app: mysql-app
  tier: mysql
---
apiVersion: v1 
kind: Deployment            
metadata:
  name: mysql-deployment       
  labels:                       
    app: mysql-app 
spec:
  selector:
    matchLabels:
      app: mysql-app
    tier: mysql 
  strategy:
    type: Recreate
  template:                    
    metadata:
      labels:                  
        app: mysql-app
      tier: mysql 
    spec:                       
      containers: 
      - images: mysql:5.6 
        name: mysql
        env:                        
        - name: MYSQL_ROOT_PASSWORD 
          valueFrom:                
          secretKeyRef: 
            name: mysql-root-pass 
              key: password 
        - name: MYSQL_DATABASE
          valueFrom:
          secretKeyRef: 
            name: mysql-db-url 
              key: database 
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        ports:
        - containerPort: 3306        
          name: mysql
        volumeMounts:
        - name: mysql-pv 
          mountPath: /var/lib/mysql
      volumes:                       
      - name: mysql-pv
          persistentVolumeClaim:
          claimName: mysql-pv-claim 
   ```

2. **Manifest Inspection:**
   To address the errors, we need to inspect and correct the `mysql_deployment.yml` manifest file.

   Since the manifest employs secrets as injected environment variables, it's crucial to verify the existence of the required secrets.

```shell
   $ kubectl get secrets
   ```

   This command will list the available secrets, which should include entries like `mysql-db-url`, `mysql-root-pass`, and `mysql-user-pass`.

3. **Manifest Corrections:**
   There's no need to split the manifest into separate spec files for each resource; instead, we can rectify the provided YAML file directly. Here are the necessary corrections:

   - Indent "tier: mysql."
   - Update "mysql-pv" to be the persistent volume name.
   - Indent "app: mysql-app" under PVC.
   - Indent "type: local" under PV.
   - Change "apps/v1" under PV and PVC to "v1."
   - Indent "path: "/mnt/data"" under PV.
   - Ensure "persistentVolumeReclaimPolicy" under PV has a string value.
   - Update "storage: 250MB" under PVC to "250Mi."
   - Correct the indentation of attributes under "volumes" under Deployment.
   - Indent "claimName: mysql-pv-claim" under Deployment.
   - Change "images: mysql:5.6" under Deployment to "image."

   The corrected manifest should resemble the following:

   ```yaml
   mysql_deployment.yml
   apiVersion: v1 
   kind: PersistentVolume            
   metadata:
     name: mysql-pv
     labels: 
       type: local 
   spec:
     storageClassName: standard      
     capacity:
       storage: 250Mi
     accessModes: 
       - ReadWriteOnce
     hostPath:                       
       path: "/mnt/data" 
     persistentVolumeReclaimPolicy: Retain  
   ---
   apiVersion: v1 
   kind: PersistentVolumeClaim 
   metadata:                          
     name: mysql-pv-claim
     labels:
       app: mysql-app 
   spec:                              
     storageClassName: standard       
     accessModes:
       - ReadWriteOnce                
     resources:
       requests: 
         storage: 250Mi
   ---
   apiVersion: v1                    
   kind: Service                      
   metadata:
     name: mysql         
     labels:             
       app: mysql-app
   spec:
     type: NodePort
     ports:
       - targetPort: 3306
         port: 3306
         nodePort: 30011
     selector:    
       app: mysql-app
       tier: mysql
   ---
   apiVersion: apps/v1 
   kind: Deployment            
   metadata:
     name: mysql-deployment       
     labels:                       
       app: mysql-app 
   spec:
     selector:
       matchLabels:
         app: mysql-app
         tier: mysql 
     strategy :
       type: Recreate
     template:                    
       metadata:
         labels:                  
           app: mysql-app
           tier: mysql 
       spec:                       
         containers: 
         - image: mysql:5.6 
           name: mysql
           env:                        
           - name: MYSQL_ROOT_PASSWORD 
             valueFrom:                
               secretKeyRef: 
                 name: mysql-root-pass 
                 key: password 
           - name: MYSQL_DATABASE
             valueFrom:
               secretKeyRef: 
                 name: mysql-db-url 
                 key: database 
           - name: MYSQL_USER
             valueFrom:
               secretKeyRef:
                 name: mysql-user-pass
                 key: username
           - name: MYSQL_PASSWORD
             valueFrom:
               secretKeyRef:
                 name: mysql-user-pass
                 key: password
           ports:
           - containerPort: 3306        
             name: mysql
           volumeMounts:
           - name: mysql-persistent-storage 
             mountPath: /var/lib/mysql
         volumes:                       
         - name: mysql-persistent-storage
           persistentVolumeClaim:
             claimName: mysql-pv-claim
   ```

4. **Apply the Manifest:**
   
   Apply the corrected manifest to the Kubernetes cluster.

   ```shell
   kubectl apply -f .
   ```

5. **Resource Verification:**

   Verify that the resources have been successfully created.

```shell
   $ kubectl get pods
   $ kubectl get svc
   $ kubectl get pv
   $ kubectl get pvc
   ```

6. **Log Inspection:**
   
   Finally, you can check the logs to ensure that the MySQL deployment is running correctly.

```shell
   $ kubectl logs -f deployment/mysql-deployment
   ```

   You should see log entries indicating the status of the MySQL deployment.

This series of steps will guide you through deploying and troubleshooting a MySQL deployment in a Kubernetes cluster, starting from an incorrect manifest file.