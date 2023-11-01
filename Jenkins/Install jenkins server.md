The DevOps team of xFusionCorp Industries is planning to setup some CI/CD pipelines. After several meetings they have decided to use Jenkins server. So, we need to setup a Jenkins Server as soon as possible. Please complete the task as per requirements mentioned below:



1. Install jenkins on jenkins server using yum utility only, and start its service. You might face timeout issue while starting the Jenkins service, please refer this link for help.


2. Jenkin's admin user name should be theadmin, password should be Adm!n321, full name should be Anita and email should be anita@jenkins.stratos.xfusioncorp.com.


Note:


1. For this task, ssh into the jenkins server using user root and password S3curePass from jump host.


2. After installing the Jenkins server, please click on the Jenkins button on the top bar to access Jenkins UI and follow the on-screen instructions to create an admin user.






------------------

**update and install wget if not already installed**
```bash
yum update -y
yum install wget -y 
```

```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo dnf upgrade
# Add required dependencies for the jenkins package
sudo dnf install java-17-openjdk
sudo dnf install jenkins
sudo systemctl daemon-reload
```

**start the service**

```bash
sudo systemctl start jenkins
```

open the jenkins GUI and wait for it to start

**Retrieve the initial jenkins password**
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

**Install plugins and Create a new user account**

This is based on the project specifications for the given user details
