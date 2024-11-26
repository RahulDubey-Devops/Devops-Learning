# DAY-4 | JENKINS

## Introduction to Jenkins
Jenkins is an open-source automation server that streamlines tasks in the software development lifecycle, such as building, testing, and deploying applications. It features a web-based interface and supports numerous plugins for integration with various tools and technologies.

### JENKIN
![image](https://github.com/user-attachments/assets/219f7f4d-4189-4529-8cee-b357fa00117b)
---

## Real-World Scenario
Jenkins is pivotal in modern software development workflows. Here's a typical use case:

1. **Version Control**: Developers push code changes to a version control system (e.g., Git).
2. **Continuous Integration**: Jenkins monitors the repository for changes and triggers builds automatically.
3. **Build**: Jenkins compiles the code, runs unit tests, and creates build artifacts.
4. **Automated Testing**: Jenkins runs various tests like unit, integration, and functional tests.
5. **Code Quality Analysis**: Tools like SonarQube or Checkstyle evaluate the code quality.
6. **Artifact Storage**: Build artifacts are archived for deployment and distribution.
7. **Deployment**: Jenkins deploys artifacts to staging or production environments.
8. **Notification and Reporting**: Jenkins notifies stakeholders about build/test statuses and generates reports.
9. **Continuous Deployment**: Automates production deployments with predefined conditions.
10. **Monitoring and Scaling**: Integrates with monitoring tools and scales infrastructure as required.
11. **Scheduled Jobs**: Automates recurring tasks like backups and maintenance.

Jenkins ensures automation, improves code quality, and enhances collaboration among team members.

---

## Ways to Install Jenkins on Windows and Linux

### Install Jenkins on Linux - Method 1
```bash
sudo apt update -y
sudo apt install openjdk-11-jre -y

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y 
sudo apt-get install jenkins -y

sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```
### Install Jenkins on Linux - Method 2
https://www.jenkins.io/doc/book/installing/linux/

# Jenkins Guide

## Installing Plugins
1. Navigate to **Manage Jenkins > Manage Plugins**.
2. Go to the **Available** tab to find and install the desired plugins.
3. Click **Install without restart** or restart Jenkins if prompted.

---

## Global Tool Configuration
1. Navigate to **Manage Jenkins > Global Tool Configuration**.
2. Add and configure tools such as:
   - JDK
   - Git
   - Maven

---

## Configure System
1. Navigate to **Manage Jenkins > Configure System**.
2. Update configurations like:
   - Jenkins URL
   - Email notifications
   - Security settings

---

## Adding Credentials
1. Navigate to **Credentials > System > Global credentials**.
2. Add credentials with appropriate types (e.g., Username/Password, SSH Keys) and descriptions.

---

## Jenkins Job Tutorial

### Freestyle Job
1. **Create a Freestyle Project**:
   - From the Jenkins dashboard, click **New Item**, enter a name, and select **Freestyle Project**.
2. **Configure Git Repository**:
   - Go to the **Source Code Management** section and enter the Git repository details.
3. **Add Build Steps**:
   - In the **Build** section, add a shell script and use the following commands:
     ```bash
     #!/bin/bash
     git checkout <branch_name>
     mvn compile
     mvn package
     cp target/webapp.war /path/to/tomcat/webapps/
     ```

---

### Pipeline Job
1. **Create a Pipeline Project**:
   - From the Jenkins dashboard, click **New Item**, enter a name, and select **Pipeline**.
2. **Configure Pipeline Script**:
   - In the **Pipeline** section, use the following script:
     
     ```groovy
    pipeline {
    agent any

   tools {
    jdk 'jdk11'   // Ensure JDK 21 is configured in Global Tool Configuration
    maven 'maven3' // Ensure Maven 3 is configured in Global Tool Configuration
  }
  
   environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
      stage("Git Checkout"){
          steps{
                git branch: 'main', url: 'https://github.com/RahulDubey-Devops/Petclinic-demo.git'
          }
      }
      stage("Maven Compile"){
          steps{
              sh 'mvn clean compile'
          }
      }
      stage("test"){
          steps{
              sh 'mvn test'
          }
      }
      
stage('SonarQube Analysis') {
    steps {
        // Make sure you use the correct SonarQube server environment as defined in Jenkins configuration
        withSonarQubeEnv('sonar-server') {
         sh 'mvn sonar:sonar'
        }
    }
}
      stage('Maven Package') {
          steps {
            // Package the project
            sh 'mvn package'
          }
        }
          stage("Deplo to Tomcat"){
            steps{
           // Switch to root user and provide the password for su
            sh "sudo cp target/*.war /opt/tomcat/webapps/"
            }
        }
}
---


![image](https://github.com/user-attachments/assets/69e9cebd-b344-4bb6-b934-11e36d7c8cb4)


![image](https://github.com/user-attachments/assets/ae035ed4-48cc-41ed-837b-d6a2e378aeda)


### Multibranch Pipeline
1. **Create a Multibranch Pipeline Project**:
   - From the Jenkins dashboard, click **New Item**, enter a name, and select **Multibranch Pipeline**.
2. **Configure Branch Sources**:
   - Add your Git repository and credentials.
3. **Pipeline Script**:
   - Use the same script as mentioned in the **Pipeline Job** section.

---



🚀 **Start automating your software development workflow with Jenkins!** 🚀


