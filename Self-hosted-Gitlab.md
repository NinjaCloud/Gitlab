# Configuring Your Own GitLab Instance

## GitLab Introduction:
In this guide, we will cover how to install and configure GitLab on an Ubuntu 22.04 server.

### Pre-Requisites
- Ubuntu 22.04 Server
- 8 GB RAM Min or Max
- Processor 2 cores
- Storage – 10 GB
- DNS – (We are using gitlab.owndomain.com or server IP)

### Step 1: Installing the Dependencies and Package
Before we install GitLab, it is essential to install some of the software that it leverages during installation and on an ongoing basis. 

Set HostName
```
sudo hostnamectl set-hostname GitLabs
bash
```
Fortunately, all the required software can be easily installed from Ubuntu’s default package repositories.
```bash
sudo apt update
sudo apt install ca-certificates curl postfix
```

### Step 2: Installing GitLab
Leverage the installation script to configure your system with the GitLab repositories. 
```
curl -LO https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh
```
```
sudo bash script.deb.sh
```
```
sudo apt install gitlab-ce
```
Reconfigure
```
sudo gitlab-ctl reconfigure
```

### Step 3: Performing Initial Config through Web Interface
Logging in for the first time. Visit the  IP address of your GitLab server in your web browser:

http://xxx.xxx.xxx.xxx

On your first time visiting, you should see an initial prompt to set a password for the administrative account. If not follow the below steps.

### Step 4: Open a Rails Console

Use the following command to open a Rails console:
```
sudo gitlab-rails console
```
Find the Root User:
```
user = User.where(id: 1).first
```
Reset the Password:
```
user.password = 'your_new_password'
```
```
user.password_confirmation = 'your_new_password'
```
```
user.save!
```
Replace 'your_new_password' with your desired password.

Exit the Rails Console:
```
exit
```
Restart GitLab (optional but recommended):

Restart GitLab to ensure all services are refreshed:
```
sudo gitlab-ctl restart
```
