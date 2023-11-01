The Nautilus production support team and security team had a meeting last month in which they decided to use local yum repositories for maintaing packages needed for their servers. For now they have decided to configure a local yum repo on Nautilus Backup Server. This is one of the pending items from last month, so please configure a local yum repository on Nautilus Backup Server as per details given below.



a. We have some packages already present at location /packages/downloaded_rpms/ on Nautilus Backup Server.

b. Create a yum repo named local_yum and make sure to set Repository ID to local_yum. Configure it to use package's location /packages/downloaded_rpms/.

c. Install package vim-enhanced from this newly created repo.

To configure a local yum repository on the Nautilus Backup Server, follow these steps:

1. Verify the presence of packages in the specified location:
```
$ ls /packages/downloaded_rpms/
```
Make sure that the packages are present in the directory.

2. Create a repository configuration file:
```
$ sudo vi /etc/yum.repos.d/local_yum.repo
```
Add the following lines to the file:
```
[local_yum]
name=Local Yum Repository
baseurl=file:///packages/downloaded_rpms/
enabled=1
gpgcheck=0
```
Save and exit the file.

3. Clear the yum cache:
```
$ sudo yum clean all
```

4. Update the yum repositories:
```
$ sudo yum update
```

5. Install the vim-enhanced package from the newly created repository:
```
sudo yum --disablerepo="*" --enablerepo="local_yum" list available
```

```
sudo yum --disablerepo="*" --enablerepo="local_yum" install vim-enhanced
```
Yum will now install the vim-enhanced package from the local repository.
6. Ensure that the package is successfully installed and accessible for use. This confirms where the package is installed from, the local_yum repo wil be identified as @local_yum

```
sudo yum list vim-enhanced
```

