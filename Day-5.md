# DAY-5 | SonarQube Guide

## What is SonarQube?

SonarQube is an open-source platform designed for static code analysis and code quality management. It helps developers identify bugs, vulnerabilities, code smells, and code duplications early in the software development lifecycle. SonarQube also provides actionable insights to maintain high-quality codebases.

---
---
## Key Features of SonarQube:

1. **Static Code Analysis**  
   - Analyzes source code structure and syntax for potential issues.
   
2. **Code Quality Metrics**  
   - Tracks metrics like code coverage, cyclomatic complexity, duplications, and maintainability index.

3. **Issue Tracking**  
   - Categorizes issues as bugs, vulnerabilities, or code smells, with recommendations for fixes.

4. **Continuous Inspection**  
   - Integrates with CI/CD workflows for automated code analysis.

5. **Custom Rules and Quality Profiles**  
   - Allows defining project-specific coding standards and rules.

6. **Integration Capabilities**  
   - Compatible with IDEs, build tools, and CI/CD platforms for real-time feedback and automation.

---

## Docker Installation for SonarQube

### Install Docker:
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo apt install docker-compose

service docker restart
sudo usermod -aG docker $USER
newgrp docker
sudo chmod 666 /var/run/docker.sock
sudo systemctl restart docker
```

### Install SonarQube using Docker:
```
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```
# Real World Scenario: Code Quality Assessment and Continuous Improvement

## Scenario:
A development team integrates SonarQube into their workflow to ensure code quality.

### Steps:

1. **Integration with CI/CD Pipeline**  
   - Trigger SonarQube analysis automatically after code compilation.

2. **Static Code Analysis**  
   - Scans the codebase for issues like bugs, vulnerabilities, and code smells.

3. **Quality Reports and Dashboards**  
   - Provides insights into code quality metrics and trends over time.

4. **Issue Management**  
   - Categorizes issues by severity for prioritization.

5. **Code Review and Collaboration**  
   - Facilitates team discussions for code improvement.

6. **Enforcement of Coding Standards**  
   - Ensures consistent quality with predefined rules.

7. **Continuous Improvement**  
   - Tracks and resolves recurring issues for enhanced maintainability.

8. **Monitoring Trends**  
   - Analyzes trends to identify improvements or regressions in code quality.

---


markdown
Copy code
# Real World Scenario: Code Quality Assessment and Continuous Improvement

## Scenario:
A development team integrates SonarQube into their workflow to ensure code quality.

### Steps:

1. **Integration with CI/CD Pipeline**  
   - Trigger SonarQube analysis automatically after code compilation.

2. **Static Code Analysis**  
   - Scans the codebase for issues like bugs, vulnerabilities, and code smells.

3. **Quality Reports and Dashboards**  
   - Provides insights into code quality metrics and trends over time.

4. **Issue Management**  
   - Categorizes issues by severity for prioritization.

5. **Code Review and Collaboration**  
   - Facilitates team discussions for code improvement.

6. **Enforcement of Coding Standards**  
   - Ensures consistent quality with predefined rules.

7. **Continuous Improvement**  
   - Tracks and resolves recurring issues for enhanced maintainability.

8. **Monitoring Trends**  
   - Analyzes trends to identify improvements or regressions in code quality.

---


### Jenkins Pipeline for SonarQube Analysis:
Pipeline Script:
```groovy

pipeline {
    agent any
    
    tools {
        jdk 'jdk11'
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/RahulDubey-Devops/Petclinic-demo.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                sh 'sudo cp target/*.war /opt/tomcat/webapps/'
            }
        }
    }
}
```
![image](https://github.com/user-attachments/assets/c0bdb515-a35d-475d-a750-14bad48bdcdd)

![image](https://github.com/user-attachments/assets/607b843e-93a3-4ecd-86c5-d709834bcb13)


