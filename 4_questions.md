# CICD/Jenkins Interview Questions

## Question 1: How do you handle secrets?

Handling secrets securely is crucial for any CI/CD pipeline.

1. **Platform-Specific Methods:**
- For GitHub: Store secrets securely in GitHub Actions.
- For GitLab: Use GitLab’s CI/CD variables for secrets.

2. **Cloud-Based Solutions:**
- AWS: Use AWS Systems Manager or Secrets Manager.
- Azure: Store secrets in Azure Key Vault.
- Independent Tools: Tools like HashiCorp Vault offer robust secret management for any environment.

3. **Jenkins:**
- Credentials Plugin: Jenkins provides a credentials plugin that can be used to store secrets such as passwords, API keys, and certificates. The secrets are encrypted and stored securely within Jenkins, and can be easily retrieved in build scripts or used in other plugins.
- Environment Variables: Secrets can be stored as environment variables in Jenkins and referenced in build scripts. However, this method is less secure because environment variables are visible in the build logs.

_________________________________________________________________________________________

## Question 2: What is your deployment strategy?

1. **Blue-Green Deployment:**
- Deploy the new version alongside the current version (e.g., v34 and v35).
- Redirect traffic to v35 via a load balancer after testing.
- Roll back easily by pointing the load balancer back to v34 if needed.

2. **Canary Deployment:**
- Gradually introduce v35 by routing 10%, then 30%, and finally 100% of traffic.
- Use weighted load balancers like NGINX or AWS Application Load Balancer for ratio-based traffic distribution.

____________________________________________________________________________________

## Question 3: What do you do if a deployed application is faulty?

- With Blue-Green Deployment, rolling back is seamless: redirect traffic back to the previous version.
- If using Canary Deployment, halt the rollout and restore traffic distribution to the stable version.

Highlight any automated monitoring tools (like Prometheus) that detect and alert you to application issues.

______________________________________________________________________________________

## Question 4: How do you manage Jenkins backups and scaling?

1. **Scaling:** In our setup, Jenkins runs on AWS EC2 instances managed via Auto Scaling Groups. Predictive scaling ensures we handle build surges seamlessly. New nodes connect using SSH for immediate integration."

2. **Backups:** Backing up Jenkins is a very easy process, there are multiple default and configured files and folders in Jenkins that you might want to backup.

- Configuration: The `~/.jenkins` folder. You can use a tool like rsync to backup the entire directory to another location.
- Plugins: Backup the plugins installed in Jenkins by copying the plugins directory located in JENKINS_HOME/plugins to another location.
- Jobs: Backup the Jenkins jobs by copying the jobs directory located in JENKINS_HOME/jobs to another location.
- User Content: If you have added any custom content, such as build artifacts, scripts, or job configurations, to the Jenkins environment, make sure to backup those as well.
- Database Backup: If you are using a database to store information such as build results, you will need to backup the database separately. This typically involves using a database backup tool, such as mysqldump for MySQL, to export the data to another location.

One can schedule the backups to occur regularly, such as daily or weekly, to ensure that you always have a recent copy of your Jenkins environment available. You can use tools such as cron or Windows Task Scheduler to automate the backup process.

_______________________________________________________________________________________

## Question 5: Branching Strategy:

#### 1. Feature Branches:
- Used for individual feature development.
- Can be created and discarded as needed.

#### 2. Main (Master) Branch:
- Houses the most up-to-date stable code.
- Where active development happens and all features are merged once they are complete.

#### 3. Release Branch:
- Used to prepare a new release for production.
- The release branch allows for final fixes and changes after feature development is complete and integrated.

#### 4. Hotfix Branch:
- A critical fix for an issue identified in production.
- A hotfix is made directly on a release branch (e.g., for iOS version 15) and merged back into the main and release branches.

________________________________________________________________________________________

## Question 6: CICD Tools

1. **Jenkins:**
- Acts as the orchestrator for CI/CD pipelines.
- Triggers different pipelines for each stage (feature, staging, release, production).
- Ensures automation of stages like code build, tests, Docker image creation, and deployment.

2. **Argo CD:**
- Manages the deployment to different Kubernetes clusters (for development, staging, pre-production, and production environments).
- Uses GitOps for continuous deployment by watching a Git repository for changes and syncing them with the Kubernetes clusters.

3. **Kubernetes:**
- Each environment (development, staging, pre-production, and production) is hosted on a separate Kubernetes cluster for better isolation.
- Namespaces within clusters could be used for managing different environments in some cases, though separate clusters are generally recommended.

________________________________________________________________________________________

## Question 7: What are the different ways to trigger jenkins pipelines ?

1. **Poll SCM:** 
- Jenkins can periodically check the repository for changes and automatically build if changes are detected. 
- This can be configured in the "Build Triggers" section of a job.
                 
2. **Build Triggers:** 
- Jenkins can be configured to use the Git plugin, which allows you to specify a Git repository and branch to build. 
- The plugin can be configured to automatically build when changes are pushed to the repository.
                 
3. **Webhooks:** 
- A webhook can be created in GitHub to notify Jenkins when changes are pushed to the repository. 
- Jenkins can then automatically build the updated code. This can be set up in the "Build Triggers" section of a job and in the GitHub repository settings.

________________________________________________________________________________________

## Question 8: What is latest version of Jenkins or which version of Jenkins are you using ?

This is a very simple question interviewers ask to understand if you are actually using Jenkins day-to-day, so always be prepared for this.

________________________________________________________________________________________

## Question 9: What is JNLP and why is it used in Jenkins ?

In Jenkins, JNLP is used to allow agents (also known as "slave nodes") to be launched and managed remotely by the Jenkins master instance. This allows Jenkins to distribute build tasks to multiple agents, providing scalability and improving performance.

When a Jenkins agent is launched using JNLP, it connects to the Jenkins master and receives build tasks, which it then executes. The results of the build are then sent back to the master and displayed in the Jenkins user interface.

________________________________________________________________________________________

## Question 10: What are some of the common plugins that you use in Jenkins ?

Be prepared for answer, you need to have atleast 3-4 on top of your head, so that interview feels you use jenkins on a day-to-day basis.

________________________________________________________________________________________

## Question 11: How to add a new worker node in Jenkins ?

Log into the Jenkins master and navigate to Manage Jenkins > Manage Nodes > New Node. Enter a name for the new node and select Permanent Agent. Configure SSH and click on Launch.

________________________________________________________________________________________

## Question 12:  How to add a new plugin in Jenkins ?

Using the CLI, ```java -jar jenkins-cli.jar install-plugin <PLUGIN_NAME>```
  
Using the UI,
- Click on the "Manage Jenkins" link in the left-side menu.
- Click on the "Manage Plugins" link.

_______________________________________________________________________________________

## Question 13: What is shared modules in Jenkins ?

Shared modules in Jenkins refer to a collection of reusable code and resources that can be shared across multiple Jenkins jobs. This allows for easier maintenance, reduced duplication, and improved consistency across multiple build processes.

For example, shared modules can be used in cases like:
- Libraries: Custom Java libraries, shell scripts, and other resources that can be reused across multiple jobs.
- Jenkinsfile: A shared Jenkinsfile can be used to define the build process for multiple jobs, reducing duplication and making it easier to manage the build process for multiple projects.
- Plugins: Common plugins can be installed once as a shared module and reused across multiple jobs, reducing the overhead of managing plugins on individual jobs.
- Global Variables: Shared global variables can be defined and used across multiple jobs, making it easier to manage common build parameters such as version numbers, artifact repositories, and environment variables.

_____________________________________________________________________________________________

## Question 14: Can you use Jenkins to build applications with multiple programming languages using different agents in different stages ?

- Yes, Jenkins can be used to build applications with multiple programming languages by using different build agents in different stages of the build process.
- Jenkins supports multiple build agents, which can be used to run build jobs on different platforms and with different configurations. By using different agents in different stages of the build process, you can build applications with multiple programming languages and ensure that the appropriate tools and libraries are available for each language.
- For example, you can use one agent for compiling Java code and another agent for building a Node.js application. The agents can be configured to use different operating systems, different versions of programming languages, and different libraries and tools.
- Jenkins also provides a wide range of plugins that can be used to support multiple programming languages and build tools, making it easy to integrate different parts of the build process and manage the dependencies required for each stage.
- Overall, Jenkins is a flexible and powerful tool that can be used to build applications with multiple programming languages and support different stages of the build process.

_____________________________________________________________________________________________

## Question 15: How to setup auto-scaling group for Jenkins in AWS ?

Here is a high-level overview of how to set up an autoscaling group for Jenkins in Amazon Web Services (AWS):

- Launch EC2 instances: Create an Amazon Elastic Compute Cloud (EC2) instance with the desired configuration and install Jenkins on it. This instance will be used as the base image for the autoscaling group.
- Create Launch Configuration: Create a launch configuration in AWS Auto Scaling that specifies the EC2 instance type, the base image (created in step 1), and any additional configuration settings such as storage, security groups, and key pairs.
- Create Autoscaling Group: Create an autoscaling group in AWS Auto Scaling and specify the launch configuration created in step 2. Also, specify the desired number of instances, the minimum number of instances, and the maximum number of instances for the autoscaling group.
- Configure Scaling Policy: Configure a scaling policy for the autoscaling group to determine when new instances should be added or removed from the group. This can be based on the average CPU utilization of the instances or other performance metrics.
- Load Balancer: Create a load balancer in Amazon Elastic Load Balancer (ELB) and configure it to forward traffic to the autoscaling group.
- Connect to Jenkins: Connect to the Jenkins instance using the load balancer endpoint or the public IP address of one of the instances in the autoscaling group.
- Monitoring: Monitor the instances in the autoscaling group using Amazon CloudWatch to ensure that they are healthy and that the autoscaling policy is functioning as expected.

By using an autoscaling group for Jenkins, you can ensure that you have the appropriate number of instances available to handle the load on your build processes, and that new instances can be added or removed automatically as needed. This helps to ensure the reliability and scalability of your Jenkins environment.

_____________________________________________________________________________________________

## Question 16: Explain the CI/CD Process

The standard flow typically involves:

- **Code Checkout:** We start by checking out the code from version control (e.g., Git).
- **Build:** We then build the application.
- **Code Testing:** Next, we test the code for vulnerabilities, perform static code analysis, and security checks.
- **Storing Artifacts:** After testing, we store the artifacts (build output).
- **Deployment:** Finally, we deploy the application onto a Kubernetes cluster using tools like Argo CD (or Ansible, depending on your preference).

___________________________________________________________________________________________

## Question 17: How do you handle issues in your worker nodes? If your Jenkins jobs are running on worker nodes, how do you manage them if they go down or become unresponsive?

- **Initial Troubleshooting:** You can start by logging into the worker node to understand the issue. You might check the node's health, view thread dumps, or look for resource utilization problems (like CPU or RAM).
- **Proactive Monitoring:** A Python application to monitor the health of worker nodes. For example, if CPU or RAM usage exceeds 80%, the application sends you an alert.
- **Auto-Scaling:** Setting up auto-scaling on EC2 instances to handle traffic spikes.
- **Docker Agents:** The best approach is using Docker agents. Jenkins can spin up containers as needed for jobs, ensuring that the Jenkins environment is never down and can always accept new jobs.

______________________________________________________________________________________________

## Question 18: What kind of Worker-agents are used in Jenkins?

Jenkins uses the following types of agents:

- Static Agents: Fixed, always-available agents (e.g., physical or virtual machines).
- Dynamic Agents: Provisioned on-demand, often in cloud environments (e.g., AWS EC2, Kubernetes).
- Permanent Agents: Continuously connected, suitable for frequent jobs.
- Ephemeral Agents: Temporary agents created for specific jobs and terminated afterward.
- Cloud Agents: Provisioned in cloud environments (e.g., AWS, GCP, Kubernetes).
- Docker Agents: Run jobs in Docker containers for specific environments.
- Self-Hosted Agents: Managed and configured by users on their own infrastructure.
- Swarm Agents: Lightweight agents connected via the Swarm Plugin for dynamic systems.

When you use **Docker agents** in Jenkins, one of the key benefits is that you don’t have to worry about installing and configuring tools on the Jenkins agent itself. Docker provides a clean, isolated environment for each job, and you can define the environment as part of your Jenkins pipeline configuration. Here's how it helps:

Docker agents greatly simplify the setup and configuration of build environments in Jenkins. By using Docker, you can avoid installing tools and managing dependencies on individual agents, providing a more efficient, scalable, and consistent build process.

_____________________________________________________________________________________________

## Question 19: Github Webhooks Workflow

Jenkins doesn't actively "watch" GitHub repositories. Instead, GitHub webhooks are used to notify Jenkins of changes in the repository.

- A webhook is configured in the GitHub repository settings to point to a specific URL on the Jenkins server (typically ```<Jenkins_URL>/github-webhook/```).
- The webhook is configured to listen to specific events, such as: Push events (e.g., commits to a branch) or Pull request events (e.g., opened, updated, or merged).
- When one of the specified events occurs, GitHub sends an HTTP POST request to the Jenkins webhook URL with details of the event (like branch name, commit hash, etc.).
- Jenkins processes the webhook payload and triggers the appropriate job based on the event. For example: A job might be set up to build the code whenever there's a push to the main branch. Another job might be set up to run tests or deploy on a pull request merge.
- Once triggered, Jenkins pulls the latest changes from the repository using the Git plugin or a similar SCM integration and proceeds with the pipeline or build process.

**Key Points:**
- Webhook-driven notifications ensure that Jenkins operates on an event-based system rather than polling the GitHub repository at intervals, which can save resources and provide faster response times.
- The webhook payload carries the data necessary for Jenkins to decide which job to execute and what parameters to use.

______________________________________________________________________________________________

## Question 20: useradd | adduser

```useradd```
- A low-level command.
- Usage: Used to add a user with minimal default settings.

```adduser```
- A higher-level command (a Perl script that utilizes useradd in the background).
- Usage: More user-friendly and interactive, suitable for most users and administrators.




