# Jenkins Pipeline using Gitops Approach

![Screenshot 2023-03-28 at 9 38 09 PM](https://user-images.githubusercontent.com/43399466/228301952-abc02ca2-9942-4a67-8293-f76647b6f9d8.png)

This setup integrates Jenkins for Continuous Integration (CI) and GitOps tools like Argo-CD and Argo-Image-Updater for Continuous Delivery (CD).

## Architecture

1. **Git Repository 1 (Source Code):** Developers push code changes that trigger Jenkins pipelines.
2. **Jenkins CI Process:** Build -> Test -> Security -> Dockerize -> Push to Container Registry.
3. **Container Registry:** Stores the Docker image (e.g., AWS ECR, Docker Hub).
4. **Git Repository 2 (Application Manifests):** Stores Kubernetes manifests, Helm charts, etc.
5. **Argo Image Updater:** Detects new images and updates Git Repository 2.
6. **Argo CD:** Monitors Git Repository 2 and syncs the Kubernetes cluster.

## Continuous Integration (CI) with Jenkins

**Step 1: Source Code Management**
- The application source code (e.g., Java application) resides in a Git repository.
- Webhooks are configured to trigger Jenkins pipelines whenever a developer raises a pull request.

**Step 2: Pipeline Execution**
- A declarative Jenkins pipeline is used for better readability and collaboration. 
- The application is built using Maven, with unit tests executed as part of this stage.
- Tools like SonarQube check for code quality issues and static vulnerabilities.
- SAST (Static Application Security Testing) and DAST (Dynamic Application Security Testing) ensure the application is free from security vulnerabilities.
- A Dockerfile stored in the repository is used to create a Docker image of the application.
- The image is then pushed to a container registry (e.g., AWS ECR, Docker Hub).
- Notifications (email, Slack, etc.) are sent for pipeline success or failure.

## Continuous Delivery (CD) with GitOps

The CD process leverages GitOps principles, ensuring that the application's desired state in Git matches the Kubernetes cluster.

**Step 1: Application Manifest Repository**
- A separate Git repository is used to store application manifests like: ```pod.yaml```, ```deployment.yaml```, Helm charts or ```values.yaml```.

**Step 2: Automation Tools**
- **Argo Image Updater:** Continuously monitors the container registry for new image versions. Automatically updates the manifest repository (e.g., Helm charts, ```deployment.yaml```) with the new image version.
- **Argo CD:** A Kubernetes controller deployed inside the Kubernetes cluster. Monitors the manifest repository and applies changes (new image versions, configurations, etc.) to the cluster.

**Step 3: Synchronization and Auditing**
- Argo CD ensures the Kubernetes cluster state aligns with the Git repository.
- If unauthorized changes are made to the cluster (e.g., manually modifying ```pod.yaml```), Argo CD reverts the changes to match the Git repository.


