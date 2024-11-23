# CICD Setup using Jenkins: 

### Installing Jenkins, configuring Docker as agent, and using pipeline script stored in a Github repository.

1. **Launch an EC2 Ubuntu instance**
- Allow inbound traffic for:
  - SSH (port 22)
  - HTTP (port 80, for Jenkins)
  - Custom TCP (port 8080, for Jenkins UI)

2. **SSH into the EC2 Instance**
```bash
ssh -i your-key.pem ubuntu@your-ec2-public-ip
```

3. **Update and Install Dependencies**
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install openjdk-11-jdk curl -y
```

4. **Install Jenkins**
```bash
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

5. **Install Docker**
```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo docker run hello-world
sudo usermod -aG docker ubuntu
sudo usermod -aG docker jenkins
```
Log out and back in to apply Docker group changes:
```bash
exit
```
SSH back into the EC2 instance.
```bash
systemctl restart docker
```

6. **Access Jenkins**
- Open Jenkins at ```http://your-ec2-public-ip:8080```.
- Retrieve the initial admin password:
  ```bash
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  ```
- Complete the Jenkins setup

7. **Install Jenkins Docker Plugin**
- Navigate to Manage Jenkins > Manage Plugins.
- Install the Docker Pipeline plugin.
- Restart Jenkins after the plugin is installed.
  ```bash
  http://<ec2-instance-public-ip>:8080/restart
  ```

8. **Create a Pipeline in Jenkins**
- Go to Jenkins UI > New Item > Pipeline > Pipeline script from SCM > SCM: Git.
- Save and trigger a build.

9. **Verify the build through worker-node (docker)**
```bash
sudo docker ps -a
```


