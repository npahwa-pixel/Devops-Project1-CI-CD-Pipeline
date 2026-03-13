# Automated CI/CD Pipeline for Web Application Deployment using Git, Jenkins, Maven, and Tomcat

## Overview

This project demonstrates a complete CI/CD pipeline for a simple Java web application. The application is built using Maven, packaged as a WAR file, deployed on Apache Tomcat, and automated through Jenkins.

The workflow begins when code is pushed to GitHub. Jenkins pulls the latest code, builds the project, archives the generated WAR artifact, backs up the currently deployed WAR, deploys the new WAR to Tomcat, and verifies the deployment by checking the application endpoint.

This project was created to understand the practical implementation of DevOps fundamentals such as source control, build automation, deployment automation, artifact management, and deployment verification.

---

## Objectives

- Understand CI/CD pipeline fundamentals
- Learn Git and GitHub workflow
- Build a Java web application using Maven
- Package the application as a WAR file
- Deploy the WAR file on Apache Tomcat
- Automate the build and deployment process using Jenkins Pipeline
- Verify deployment through the browser endpoint

---

## Tech Stack

- **Git** – version control
- **GitHub** – remote repository hosting
- **Java 21** – runtime environment
- **Maven** – build and packaging tool
- **Apache Tomcat 9** – application server
- **Jenkins** – CI/CD automation server
- **JSP** – simple web page for the sample application

---

## Architecture Workflow

```text
Developer
   |
   | Push Code
   v
GitHub Repository
   |
   | Jenkins pulls latest code
   v
Jenkins Pipeline
   |
   | Build using Maven
   v
WAR Artifact
   |
   | Deploy to Tomcat
   v
Apache Tomcat Server
   |
   | Verify Deployment
   v
Browser Endpoint

```

## Setup and Usage Guide

This section explains how another user can clone, configure, and run this project on their own system.

### 1. Clone the Repository

```bash
git clone https://github.com/npahwa-pixel/Devops-Project1-CI-CD-Pipeline.git
cd Devops-Project1-CI-CD-Pipeline
```

## Project Structure
```Devops-Project1-CI-CD-Pipeline/
├── .gitignore
├── Jenkinsfile
├── pom.xml
└── src/
    └── main/
        └── webapp/
            ├── index.jsp
            └── WEB-INF/
                └── web.xml
```

## File Description
```.gitignore```

Used to ignore generated files and local development files such as target/, .DS_Store, editor settings, and logs.

```Jenkinsfile```

Contains the complete pipeline definition for Jenkins.

```pom.xml```

Maven build configuration file. It defines the project, packaging type, plugins, and dependencies.

```src/main/webapp/index.jsp```

Main application page shown in the browser.

```src/main/webapp/WEB-INF/web.xml```

Standard Java web application deployment descriptor.

## Application URL
After deployment, the application is available at:
```http://localhost:8080/hello-webapp```

## Prerequisites
The following tools must be installed before running this project:

- Git

- Java 21

- Maven

- Apache Tomcat 9

- Jenkins

## Verify Installed Tools
```
git --version
java -version
mvn -version
```
## Environment Setup
### Java Configuration
Java 21 should be active before building the project.

Use this command in the terminal session:

```export JAVA_HOME=$(/usr/libexec/java_home -v 21)
export PATH="$JAVA_HOME/bin:$PATH"
```
To make it permanent on macOS:
```
echo 'export JAVA_HOME=$(/usr/libexec/java_home -v 21)' >> ~/.zshrc
echo 'export PATH="$JAVA_HOME/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

## Building the WAR File
The project is packaged using Maven.
## Command
```mvn clean package```
## Output
```target/hello-webapp.war``` 

## Manual Deployment to Tomcat
Before automating the deployment with Jenkins, the application was first deployed manually.
## Start Tomcat
```TOMCAT_HOME="/opt/homebrew/opt/tomcat@9/libexec"
"$TOMCAT_HOME/bin/startup.sh"
```
## Deploy the WAR
```
cp target/hello-webapp.war "$TOMCAT_HOME/webapps/"
```
## Verify Deployment
Open the following URL in the browser:
```
http://localhost:8080/hello-webapp
```
## Jenkins Setup
Jenkins was started locally using the WAR file.
### Start Jenkins
```
mkdir -p ~/jenkins
cd ~/jenkins
curl -L -o jenkins.war https://get.jenkins.io/war-stable/latest/jenkins.war
java -jar jenkins.war --httpPort=8081
```
### Access Jenkins
```
http://localhost:8081
```
### Initial Jenkins Setup
- Unlock Jenkins using the initial admin password

- Install suggested plugins

- Create the first admin user

- Open the Jenkins dashboard

### Unlock Jenkins Password
```
cat /Users/<your-username>/.jenkins/secrets/initialAdminPassword
```
### Jenkins Job Configuration
A Jenkins Pipeline job was created using the following setup:

- Job Type: Pipeline

- Definition: Pipeline script from SCM

- SCM: Git

- Repository URL: GitHub repository URL

- Branch Specifier: */main

- Script Path: Jenkinsfile

### Run the Pipeline
Click Build Now.

The pipeline will:

- fetch code from GitHub

- build the WAR using Maven

- archive the WAR artifact

- back up the existing deployed WAR

- deploy the new WAR to Tomcat

- verify that the deployed application is accessible

### Verify in Browser
After successful build:
```
http://localhost:8080/hello-webapp
```
