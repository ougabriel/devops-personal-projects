The DevOps team of xFusionCorp Industries is planning to create a number of Jenkins jobs for different tasks. To easily manage the jobs within Jenkins UI they decided to create different Folders for all Jenkins jobs based on usage/nature of these jobs. Based on the requirements shared below please perform the below mentioned task:



Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.


1. There are already two jobs httpd-php and services.

2. Create a new folder called Apache under Jenkins UI.

3. Move the above mentioned two jobs under Apache folder.

Note:
1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.

2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.



--------------------


I can guide you through the steps to accomplish the task of creating a new folder in Jenkins UI and moving existing jobs into it. Since I can't interact with your system directly, I'll provide you with detailed instructions. Make sure to follow these steps carefully:

1. **Login to Jenkins:**
   - Open your web browser and navigate to your Jenkins URL (e.g., `http://your-jenkins-server`).
   - Log in using the provided username (`admin`) and password (`Adm!n321`).

2. **Install Plugins and Restart Jenkins:**
   - If you need to install plugins, navigate to "Manage Jenkins" > "Manage Plugins."
   - Go to the "Available" tab and search for the required plugins.
   - Select the checkboxes next to the plugins you want to install.
   - Click the "Download now and install after restart" button.
   - Once the installation is complete, Jenkins will prompt you to restart. Click the "Restart Jenkins when installation is complete and no jobs are running" checkbox and then the "Restart" button.

3. **Create a New Folder:**
   - In the Jenkins dashboard, click on the "+" button on the left sidebar.
   - Select "Folder" from the dropdown menu.
   - Enter "Apache" as the folder name.
   - Click the "OK" button.

   NOTE: if you can't find "folder" on this GUI page, you will need to download the plugin called "cloudbees folders plugin"

    To install the "CloudBees Folders Plugin" in Jenkins:

a. Go to "Manage Jenkins" in the Jenkins dashboard.
b. Click on "Manage Plugins."
c. In the "Available" tab, search for "CloudBees Folders Plugin."
d. Check the checkbox next to the plugin's name.
e. Restart the service

4. **Move Jobs to the New Folder:**
   - In the Jenkins dashboard, you should see the "httpd-php" and "services" jobs.
   - For each job, click on the job name.
   - In the job configuration page, locate the "Move/Copy" section.
   - Select the "Move" option.
   - In the "Into folder" field, enter "Apache" (or select it from the dropdown).
   - Click the "Save" button.

5. **Refresh the UI:**
   - After moving the jobs, refresh the Jenkins UI to ensure that the changes are reflected.

6. **Verify the Changes:**
   - Verify that the "Apache" folder contains the moved jobs.
   - Verify that the original location of the jobs is now empty.

Please note that the steps provided are based on the general process in Jenkins UI. The exact UI elements and options might vary based on your Jenkins version and installed plugins.

Remember to take screenshots or record your screen as you perform these actions so that you can share them for review if needed. If you encounter any issues or discrepancies, consider referring to Jenkins documentation or seeking assistance from your Jenkins administrator.