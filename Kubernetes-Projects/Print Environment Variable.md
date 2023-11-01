The Nautilus DevOps team is working on to setup some pre-requisites for an application that will send the greetings to different users. There is a sample deployment, that needs to be tested. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about it.


Create a pod named print-envars-greeting.

Configure spec as, the container name should be print-env-container and use bash image.

Create three environment variables:

a. GREETING and its value should be Welcome to

b. COMPANY and its value should be Nautilus

c. GROUP and its value should be Group

Use command to echo ["$(GREETING) $(COMPANY) $(GROUP)"] message.

You can check the output using kubectl logs -f print-envars-greeting command.



----------------

Sure, it looks like you want to create a Kubernetes pod with specific environment variables and a command to echo a message. Here's a step-by-step guide on how to achieve this using a Kubernetes manifest file:

1. Create a file named `print-envars-greeting.yaml` and add the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
spec:
  containers:
    - name: print-env-container
      image: bash
      command: ["bash", "-c", "echo \"$GREETING $COMPANY $GROUP\""]
      env:
        - name: GREETING
          value: "Welcome to"
        - name: COMPANY
          value: "Nautilus"
        - name: GROUP
          value: "Group"
```

2. Apply the manifest to your Kubernetes cluster using the `kubectl` command:

```sh
kubectl apply -f print-envars-greeting.yaml
```

3. To check the output of the pod, you can use the following command:

```sh
kubectl logs -f print-envars-greeting
```

This will display the echoed message containing the values of the environment variables you've set.

Remember that the YAML manifest provided here assumes that you have the necessary permissions and access to create pods in your Kubernetes cluster. Also, the image "bash" might not be available in your specific Kubernetes environment; you might need to replace it with an appropriate image that contains a Bash shell.