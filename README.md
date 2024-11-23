# CICD (Continuous Integration and Continuous Deployment/Delivery)

- Continuous Integration and Continuous Deployment (or Continuous Delivery), is a practice in software development where code changes are continuously integrated and delivered in an automated way.
- CI/CD helps teams iterate faster, reduces bugs, and enables rapid feedback on new features and fixes, benefiting both developers and end-users.
- Popular CI/CD tools include Jenkins, GitLab CI/CD, CircleCI, Travis CI, GitHub Actions, and Azure DevOps.

**Continuous Integration (CI):**
- Developers frequently merge code changes into a shared repository.
- Each integration is verified by automated tests and builds.
- CI helps catch issues early, ensuring the new code works well with the existing codebase.

**Continuous Delivery (CD):**
- Continuous Delivery takes CI a step further by automatically preparing code for release.
- The code changes go through automated tests and are ready to be deployed to production anytime.
- CD ensures the code is always in a deployable state.

**Continuous Deployment (CD):**
- Continuous Deployment is the automation of deploying code changes directly to production after - successful tests.
- It eliminates manual intervention for deployments, enabling faster delivery to end users.

_____________________________________________________________________________________________

## CI/CD Workflow

**Code Commit → Build → Test → Deploy to Staging → Approval (for Continuous Delivery) → Production Deployment**

1. **Commit & Push Code:** Developers commit code to a repository (e.g., GitHub).
2. **Automated Build & Test:** CI/CD tools build and run tests automatically.
3. **Deploy to Staging:** If tests pass, the code is deployed to a staging environment.
4. **Deploy to Production:** After further approval or automated checks, code moves to production.

_____________________________________________________________________________________________

## Jenkins

Jenkins automates various stages of the software development lifecycle, particularly testing and deployment, to streamline the development process. With Jenkins, you can run builds, trigger tests, generate reports, and deploy applications automatically whenever changes are made to the codebase. 

- **Extensibility:** Jenkins has hundreds of plugins that support building, deploying, and automating any project.
- **Distributed Builds:** Jenkins can manage multiple environments for distributed builds, testing, and deployment.
- **Declarative Pipelines:** Jenkins pipelines define and automate the CI/CD workflow in a YAML-like syntax.
- **Community Support:** Jenkins has a strong community that continuously improves and extends its capabilities.

____________________________________________________________________________________________

### Jenkins Architecture

Jenkins architecture follows a master-agent (or controller-agent) model, designed to be flexible and scalable, enabling it to handle both simple and complex workflows efficiently.

A basic structure includes:

- **Jenkins Controller:** Centralized unit for job management and reporting.
- **Static/Dynamic Agents:** Distributed workers executing tasks.
- **Source Code Repositories:** Integration with systems like GitHub or Bitbucket.
- **Build Tools:** Tools like Maven, Gradle, or npm.
- **Artifact Storage:** Location for storing build artifacts.
- **Test and Deployment Environments:** Servers or clouds for testing and deployment.

**Jenkins Controller:**
- Acts as the central brain of Jenkins.
- Creating, scheduling, and monitoring jobs.
- Hosts the GUI for user interaction.
- Delegates build/test tasks to agents.
- Stores logs, results, and artifacts.
- Limited in executing resource-heavy tasks to ensure it remains responsive.

**Jenkins Agents**
- Execute tasks delegated by the controller.
- Run builds or tests on specific platforms or environments.
- Report results back to the controller.
- *Static Agents:* Permanently connected and configured manually (VMs).
- *Dynamic Agents:* Provisioned on-demand (e.g., using Kubernetes or Docker). Docker agents are often used as ephemeral nodes, meaning they exist only for the duration of a build.

**Scaling:**
- Horizontal Scaling: Adding more agents to handle load.
- Vertical Scaling: Using a more powerful controller machine.

_____________________________________________________________________________________________

### Jenkins Workflow

- Users create and configure jobs via the Jenkins controller GUI or APIs.
- The controller assigns jobs to appropriate agents based on labels, availability, or configurations.
- Agents execute the assigned jobs and return the results to the controller.
- The controller collects results, stores build artifacts, and updates the dashboard with logs and statuses.

**Pipeline:** 
- Fetches code from GitHub
- Builds with Maven
- Tests with Selenium
- Deploys to AWS EC2

_____________________________________________________________________________________________

### Example Setup

**Integrate Jenkins with GitHub:**
- Install the GitHub plugin in Jenkins.
- Create a webhook in the GitHub repository to trigger builds on code changes.

**Create a Jenkins Job:**
- Go to Jenkins Dashboard > New Item > Pipeline.
- In the pipeline script, define the stages

**Run and Monitor:**
- Trigger the job manually or via GitHub webhook.
- Monitor the console output for progress and results.

_____________________________________________________________________________________________

### Sample Pipeline Scripts:

1. **Example-1:**
```groovy
pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/YOUR_USERNAME/YOUR_REPO.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
```
2. **Example-2:**
```groovy
pipeline {
    agent any
    stages {
        
        stage('Clone Repository') {
            steps {
                git 'https://github.com/YOUR_USERNAME/YOUR_REPO.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        
        stage('Deploy') {
            steps {
                // An example deploy command (replace with your specific method)
                sh 'scp -r . user@server:/path/to/deployment'
            }
        }
    }
}
```
3. **Example-3:**
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building application...'
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm run test'
            }
        }
        stage('Package') {
            steps {
                echo 'Creating Docker image...'
                sh 'docker build -t myapp:${env.BUILD_ID} .'
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging...'
                sh 'kubectl apply -f k8s/staging-deployment.yaml'
            }
        }
        stage('Deploy to Production') {
            when {
                input message: 'Approve deployment to Production?'
            }
            steps {
                echo 'Deploying to Production...'
                sh 'kubectl apply -f k8s/production-deployment.yaml'
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
```
_____________________________________________________________________________________________

### Jenkins Drawbacks

- **Complex Setup & Maintenance:** Jenkins configurations are complex and require frequent plugin updates.
- **High Resource Usage:** Jenkins consumes significant resources, slowing performance with many jobs.
- **Plugin Dependency:** Heavy reliance on plugins can lead to compatibility and stability issues.
- **Scalability Issues:** Not inherently scalable; difficult to manage in large, distributed environments.
- **Challenging for Beginners:** Steep learning curve due to complex interface and pipeline setup.
- **Outdated UI:** Jenkins' interface is less intuitive and outdated compared to modern CI/CD tools.
- **Limited Native Cloud Support:** Jenkins lacks native support for cloud environments, making cloud integration challenging.
- **Security Concerns:** Vulnerable to security issues, especially if plugins are outdated.

_____________________________________________________________________________________________





