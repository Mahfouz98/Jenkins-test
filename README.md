# Integration Jenkins with Github Actions

## Project Setup

### Tools Needed
- EC2 instance on AWS with Jenkins
- GitHub account

## Installation and Configuration Guide
### 1. **Prepare GitHub Repository:**
   - Create or select a GitHub repository.
   - Apply the Git Flow model.
```
# Edit your Jenkinsfile
# Save and Exit


# Push the file to GitHub Repo
$ git add Jenkinsfile
$ git commit -m "Add Jenkinsfile"
$ git push <repo> develop

# check your develop branch on github repo
```

### 2. **Installing Jenkins on EC2:**

**Adding Jenkins Repo Into Ec2**

`sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo`
    
**Adding Repo Key**

`sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key`

**Installing Dependencies (Java)**

`sudo yum install java-17-amazon-corretto-devel -y`

**Install Jenkins**

`sudo dnf install jenkins`

**Start and enable Jenkins service**

```
systemctl enable --now jenkins
sudo systemctl daemon-reload
```


### Add Inbound rule in the security group of the EC2 instance
- Type: Custom TCP
- Port Range: 8080
- source: 0.0.0.0/0

### Accessing Jenkins:
- Go to web browser and write : `http://<ec2-public-ip>:8080`
- To get Jenkins password
```bash
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

### 3. **Configure Jenkins:**
   - **Install necessary plugins in Jenkins such as Git and Pipeline.**
   - **Connect Jenkins to GitHub.**
        - Go to "Manage Jenkins" > "Manage Plugins" > "Available" and install "GitHub Integration Plugin".
     - Set up credentials in Jenkins for GitHub (username and ssh key).
   - **Create a new pipeline job.**
     - Select "New Item", name your pipeline , and choose "Pipeline" as the type.
     - In the pipeline configuration, select "Pipeline script from SCM" and choose "Git" as the SCM.
     - Enter the repository URL and credentials.
     - Specify the branch to build .
     - In Build Triggers, Check **"GitHub hook trigger for GITScm polling"**.
     - Save

### 4. **Implement Webhooks for Continuous Integration:**
**Set up webhooks in GitHub to trigger Jenkins builds on push events.**
- In GitHub, go to your repository settings and select **"Webhooks"**.
- **Add a new webhook:**
   - Payload URL: `http://<your-jenkins-url>:8080/github-webhook/`
   - Content type: **application/json**
   - Select **"Just the push event"**.
   - Ensure the webhook is active
 
### **With the webhook, Jenkins will trigger a new build every time changes are pushed to the connected branch.**

### 5. **Testing and Validation:**
   - Push a change to the `develop` branch and verify Jenkins triggers a build.
   
- Check the Jenkins dashboard for build status and output .....


![Screenshot from 2024-05-01 20-41-48](https://github.com/Mahfouz98/Jenkins-test/assets/145352617/0399c7ef-f861-4af9-b61a-ddd6761cabdc)



