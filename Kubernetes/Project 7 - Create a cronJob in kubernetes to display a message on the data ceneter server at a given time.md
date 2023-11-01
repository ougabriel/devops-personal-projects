Create a cronJob with a message
-----------------------------


There are some jobs/tasks that need to be run regularly on different schedules. Currently the Nautilus DevOps team is working on developing some scripts that will be executed on different schedules, but for the time being the team is creating some cron jobs in Kubernetes cluster with some dummy commands (which will be replaced by original scripts later). Create a cronjob as per details given below:



Create a cronjob named datacenter.


Set Its schedule to something like */6 * * * *, you set any schedule for now.


Container name should be cron-datacenter.


Use httpd image with latest tag only and remember to mention the tag i.e httpd:latest.


Run a dummy command echo Welcome to xfusioncorp!.


Ensure restart policy is OnFailure.

--------

To create the desired CronJob in Kubernetes with the specified requirements, you can follow these steps:

Step 1: Create a YAML file named `datacenter-cronjob.yaml` and add the following content:

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: datacenter
spec:
  schedule: "*/6 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: cron-datacenter
            image: httpd:latest
            command: ["echo", "Welcome to xfusioncorp!"]
          restartPolicy: OnFailure
```

Step 2: Apply the YAML file to your Kubernetes cluster using `kubectl apply`:

```bash
kubectl apply -f datacenter-cronjob.yaml
```

This will create the `datacenter` CronJob with the specified schedule and other settings. It will use the `httpd:latest` image to run the `echo` command as a dummy task every 6 minutes.

After running the above steps, the CronJob will be scheduled to run every 6 minutes and will output "Welcome to xfusioncorp!" as a dummy task. Once you have the actual scripts ready, you can replace the dummy command with the real commands that need to be executed on different schedules.

You can verify the status and details of the CronJob using `kubectl get cronjob` and `kubectl describe cronjob datacenter` commands.