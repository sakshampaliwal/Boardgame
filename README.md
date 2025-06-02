# Real-World Corporate DevOps CI/CD Pipeline 🚀

## Project Overview

This project demonstrates the creation of a complete CI/CD (Continuous Integration and Deployment) pipeline, reflecting real-world DevOps workflows. It covers infrastructure provisioning on AWS, secure Kubernetes cluster setup, Git-based source control, and end-to-end automation using Jenkins for building, testing, and deploying code. The pipeline also integrates monitoring and observability to ensure reliability in production.

A "board game" web application with full CRUD functionality is used to showcase the end-to-end software delivery process in a realistic environment.

---

## Key Features & Learnings

* **Infrastructure as Code (IaC) principles**: Setting up network, servers, and clusters.
* **Containerization & Orchestration**: Using Docker and Kubernetes for application deployment and management.
* **CI/CD Automation**: Building a comprehensive Jenkins pipeline with multiple stages.
* **Code Quality & Security**: Integrating SonarQube for static code analysis and Trivy for vulnerability scanning.
* **Artifact Management**: Using Nexus Repository for storing and versioning build artifacts.
* **Role-Based Access Control (RBAC)**: Implementing secure deployment to Kubernetes.
* **Monitoring & Observability**: Setting up application and system-level monitoring using Prometheus and Grafana.
* **Best Practices**: Emphasis on security, private environments, and industry-standard practices throughout the project.

---

## Technologies Used

* **Cloud Provider**: AWS (Amazon Web Services)
    * EC2 (Elastic Compute Cloud) for Virtual Machines
    * VPC (Virtual Private Cloud) for private network environment
    * Security Groups as firewalls
* **Version Control**: Git & GitHub (for private repository)
* **CI/CD Server**: Jenkins
* **Build Tool**: Maven (for Java application)
* **Code Quality**: SonarQube
* **Artifact Repository**: Nexus Repository OSS
* **Vulnerability Scanning**:
    * Trivy (for file system and Docker images)
    * KubeAudit (for Kubernetes cluster security)
* **Containerization**: Docker
* **Orchestration**: Kubernetes (Self-managed cluster on EC2)
* **Monitoring**:
    * Prometheus
    * Grafana
    * Blackbox Exporter (for website/HTTP monitoring)
    * Node Exporter (for system-level metrics)
* **Issue Tracking (Mentioned)**: Jira
* **Local Development/Access Tool**: MobaXterm

---

## Project Workflow

The project simulates a typical software development lifecycle:

1.  **Requirement**: A client requests a new feature or change.
2.  **Task Creation**: A Jira ticket is created to track the requirement.
3.  **Development**: A developer picks up the ticket, writes the code, and tests it locally.
4.  **Version Control**: The developer pushes the changes to a private GitHub repository.
5.  **CI/CD Pipeline Trigger**: This push automatically triggers the Jenkins pipeline.
6.  **Build & Test**: Jenkins performs compilation, unit testing, code quality checks, and vulnerability scans.
7.  **Artifact Management**: The built artifact is published to Nexus Repository.
8.  **Containerization**: A Docker image is built, scanned for vulnerabilities, and pushed to Docker Hub.
9.  **Deployment**: The application (Docker image) is deployed to the Kubernetes cluster using secure RBAC.
10. **Notification**: Jenkins sends email notifications about the pipeline's success or failure, attaching relevant reports.
11. **Monitoring**: Prometheus and Grafana continuously monitor the deployed application and the Jenkins server's health.

---

##  Project Phases

The project is structured into four main phases:

### Phase 1: Infrastructure Setup

* **Network Environment**: Create a private, isolated network using AWS VPC.
* **Kubernetes Cluster**: Set up a Kubernetes cluster with master and worker nodes on EC2 instances. Scan it with KubeAudit.
* **Virtual Machines**: Launch EC2 instances for Jenkins, SonarQube, and Nexus servers.
* **Tool Installation**: Install and configure Jenkins, SonarQube, Nexus, Docker, and monitoring tools (Prometheus, Grafana, Exporters).

### Phase 2: Source Code Management

* **Git Repository**: Create a new private repository on GitHub.
* **Push Source Code**: Upload the application's source code to this repository.

### Phase 3: CI/CD Pipeline Implementation

* **Jenkins Configuration**: Install necessary plugins (JDK, Maven, SonarQube Scanner, Docker, Kubernetes CLI). Configure global tools (JDK, Maven, SonarQube scanner, Docker).
* **Pipeline Script (Jenkinsfile)**: Write a declarative pipeline with the following stages:
    1.  **Git Checkout**: Pull code from GitHub.
    2.  **Compile**: `mvn compile`.
    3.  **Unit Test**: `mvn test`.
    4.  **File System Scan**: `trivy fs ...` to scan code and dependencies, outputting an HTML report.
    5.  **SonarQube Analysis**: Scan code with SonarQube scanner and publish results to SonarQube server.
    6.  **SonarQube Quality Gate**: Wait for SonarQube quality gate status.
    7.  **Build Application**: `mvn package` to create the application artifact.
    8.  **Publish to Nexus**: Deploy the artifact to Nexus Repository using `mvn deploy` and a `settings.xml` file configured in Jenkins.
    9.  **Build & Tag Docker Image**: `docker build -t ...` using the application's Dockerfile.
    10. **Docker Image Scan**: `trivy image ...` to scan the built Docker image, outputting an HTML report.
    11. **Push Docker Image**: `docker push ...` to Docker Hub (credentials managed by Jenkins).
    12. **Deploy to Kubernetes**:
        * Set up Kubernetes RBAC (ServiceAccount, Role, RoleBinding).
        * Store Kubernetes authentication token as a secret in Jenkins.
        * Use `kubectl apply -f deployment-service.yaml` to deploy to a specific namespace (`web apps`). The manifest defines a Deployment and a LoadBalancer Service.
    13. **Verify Deployment**: `kubectl get pods -n web apps` and `kubectl get svc -n web apps`.
* **Mail Notification**: Configure Jenkins to send email notifications on pipeline status (success/failure) using Extended E-mail Notification, attaching the Trivy image scan report.

### Phase 4: Monitoring 

* **Setup**: Create a dedicated EC2 instance for monitoring tools. Install Prometheus, Grafana, Blackbox Exporter, and Node Exporter.
* **Website Level Monitoring**:
    * Configure Prometheus to scrape metrics from **Blackbox Exporter**.
    * Blackbox Exporter probes the deployed application's URL and other target URLs (e.g., prometheus.io) for HTTP status, SSL expiry, etc.
    * Import/create a Grafana dashboard to visualize Blackbox Exporter metrics.
* **System Level Monitoring (Jenkins Server)**:
    * Install **Node Exporter** on the Jenkins server to expose system metrics (CPU, RAM, disk, network).
    * Install the **Prometheus metrics plugin** in Jenkins to expose Jenkins-specific metrics.
    * Configure Prometheus to scrape metrics from Node Exporter and the Jenkins Prometheus endpoint.
    * Import/create a Grafana dashboard (e.g., Node Exporter Full) to visualize these system metrics.

---

## Getting Started / How to Follow Along

To replicate this project:

1.  **AWS Account**: You will need an AWS account with appropriate permissions to create resources.
2.  **Prerequisites**:
    * Git Bash (or similar) installed locally for Git commands.
    * MobaXterm (or any SSH client) to connect to EC2 instances.
    * A GitHub account.
    * A Docker Hub account.
    * A Gmail account (for email notifications, with App Password enabled).
3.  **Follow Phases**: Implement the project phase by phase as outlined in this document.
4.  **Scripts & Commands**: Refer to the detailed steps and configurations described throughout this README for guidance on scripts and commands. The project provides a comprehensive structure for each stage.

*Note: Ensure you manage your AWS resources effectively to avoid unexpected costs. Clean up resources after completing the project if they are no longer needed.*

---
