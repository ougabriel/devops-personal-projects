Some new requirements have come up to install and configure some packages on the Nautilus infrastructure under Stratos Datacenter. The Nautilus DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:



Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and Adm!n321 password.


Create a Jenkins job named install-packages and configure it to accomplish below given tasks.


Add a string parameter named PACKAGE.
Configure it to install a package on the storage server in Stratos Datacenter provided to the $PACKAGE parameter.

Note:


1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also some times Jenkins UI gets stuck when Jenkins service restarts in the back end so in such case please make sure to refresh the UI page.


2. Make sure Jenkins job passes even on repetitive runs as validation may try to build the job multiple times.


3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


----------------
PREPREQUISITES
----------------
1. Install ssh plugins, ssh credentials, and ssh build
2. Configure the storage server credentials under "manage jenkins > credential"
3. Configure the storage server to use ssh under "manage jenkins > configure system > ssh remote host (ssh sites); fill in the hostname for the storage server and port type in 22 > choose credentials. Confirm connections, if successful, then save.

------------

 Below are the steps you need to follow:

1. **Log in to Jenkins:**
   - Open your web browser and go to your Jenkins URL (e.g., `http://your-jenkins-server`).
   - Log in using the provided username (`admin`) and password (`Adm!n321`).

2. **Install Plugins and Restart Jenkins (if needed):**
   - If the necessary plugins are not already installed, navigate to "Manage Jenkins" > "Manage Plugins."
   - Go to the "Available" tab and search for the required plugins.
   - Select the checkboxes next to the plugins you want to install.
   - Click the "Download now and install after restart" button.
   - Once the installation is complete, Jenkins will prompt you to restart. Click the "Restart Jenkins when installation is complete and no jobs are running" checkbox and then the "Restart" button.

3. **Create a New Jenkins Job:**
   - In the Jenkins dashboard, click on the "+" button on the left sidebar.
   - Choose "New Item" from the dropdown menu.
   - Enter "install-packages" as the job name.
   - Select "Freestyle project" and click the "OK" button.

4. **Configure the Job:**
   - Scroll down to the "Build" section.
   - Click on the "Add Parameter" dropdown and select "String Parameter."
   - Name the parameter "PACKAGE."
   - Leave the "Default Value" field empty.
   - Click the "Save" button.

5. **Add Build Step (Execute Shell via ssh):**
   - In the "Build" section, click on "Add build step" and select "Execute shell."
   - In the "Command" box, enter the command to install the package on the storage server. For example:
     ```bash
     ssh storage_server_ip "sudo apt-get install $PACKAGE -y"
     ```
     Replace `storage_server_ip` with the actual IP address of the storage server.

6. **Save and Build the Job:**
   - Scroll down and click the "Save" button.
   - Back in the job dashboard, click on "Build Now" to initiate a build for this job.

7. **Testing the Job:**
   - During the build process, Jenkins will prompt you to enter a value for the `PACKAGE` parameter.
   - Input the name of the package you want to install on the storage server. Go to the job > click build with parameter > type the package to build; Example: nginx or httpd 
   - The job will execute, attempting to install the specified package on the storage server.

