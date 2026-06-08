
## Overview
This project demonstrates a fully automated CI/CD pipeline built on a local infrastructure. It bridges the gap between version control and web serving by automatically pulling source code from a GitHub repository and deploying it to an Apache Tomcat server running locally.

This project showcases fundamental DevOps practices, including automated integration, pipeline orchestration, and environment synchronization.

## Architecture
- **Source Control:** GitHub
- **Automation Server:** Jenkins (Local Instance)
- **Web Server:** Apache Tomcat (Local Instance)
- **Deployment Mechanism:** Local shell script execution (`cp` command)

## Prerequisites
- **Operating System:** Windows
- **Jenkins:** Configured and running locally (http://localhost:8080)
- **Apache Tomcat:** Configured and running locally
- **Git:** Installed on the Jenkins host

## Pipeline Automation
The pipeline is designed for continuous synchronization without manual intervention:

- **Trigger:** Configured with **Poll SCM** using the cron expression `* * * * *`. Jenkins monitors the GitHub repository for changes every minute and triggers the pipeline automatically upon detecting new commits.
- **Workflow:** 
    1. **Checkout:** Clones the latest code from the specified GitHub branch.
    2. **Deploy:** Executes a local copy command to move web assets into the Tomcat `webapps/ROOT` directory.

## Jenkinsfile
```groovy
pipeline {
    agent any

    stages {
        stage('Fetch Source Code') {
            steps {
                echo 'Fetching latest code from GitHub...'
                git branch: 'main', url: '[https://github.com/your-username/your-repo.git](https://github.com/your-username/your-repo.git)'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying files to local Tomcat server...'
                // Ensure this path matches your local Tomcat installation
                sh 'cp -r * /opt/tomcat/webapps/ROOT/'
            }
        }
    }
}
