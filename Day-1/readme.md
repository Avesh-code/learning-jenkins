# 🚀 Jenkins Introduction 

Jenkins is an open-source automation server widely used for Continuous Integration and Continuous Delivery (CI/CD). It automates building, testing, and deploying software projects, helping teams deliver code changes quickly and reliably. ⚡️

## 🛠️ What Jenkins Does 
- Automates builds and tests whenever code is pushed.
- Integrates with tools like Git, Maven, Gradle, Selenium.
- Orchestrates deployment workflows for fast releases.

## 🧩 Core Concepts 
- **Controller**: The main Jenkins server, schedules and manages jobs.
- **Agents**: Nodes where jobs run, supporting distributed builds.
- **Jobs/Projects**: Tasks Jenkins executes (e.g., build, test, deploy).
- **Pipeline**: Workflow scripts (Jenkinsfile) that automate steps in CI/CD.

## ⭐️ Key Features 
- Cross-platform and runs anywhere Java is available.
- Web-based UI for easy setup and monitoring.
- Rich plugin ecosystem (2000+ plugins) for tool integrations.
- Build, test, and deployment monitoring with real-time feedback.

## 💡 Benefits for DevOps 
- Automates repetitive tasks for faster development cycles.
- Reduces errors and manual steps.
- Provides quick feedback on build/test status.
- Improves collaboration and visibility for teams.
---
# 🛠️ Jenkins Installation

## ✅ Prerequisites 
- Java (JDK 8 or newer) must be installed.
- Ensure you have sudo/admin privileges on your machine.

```bash
sudo apt update
sudo apt install fontconfig openjdk-21-jre
java -version
```
## 🐧 Installing Jenkins on Ubuntu/Linux 

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins
```

## check jenkins, Start and Enable jenkins

```bash
sudo systemctl status jenkins
sudo systemctl enable jenkins
```
> NOTE: If using  VM then create a rule for inbound network and allow port 8080

chack `ip-vm:8080` and get the Password from `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

---
# 🚀 Jenkins Job  

## What is a Jenkins Job?
A Jenkins job (or project) is a configured task that Jenkins executes automatically or on demand. Jobs typically automate building, testing, and deploying software.

## Basic Types of Jenkins Jobs
- **Freestyle Project:** Simple and flexible, suitable for straightforward build and test tasks.
- **Pipeline:** A scripted or declarative sequence of stages that define complex workflows, using a Jenkinsfile.
- **Multibranch Pipeline:** Pipeline per branch in a version control system.
- **Folder:** Organizes jobs into groups.

## Practical Example of a Simple Jenkins Job

### Steps Overview
1. **Create a New Freestyle Job:**  
   Define the job name and type (freestyle project).

2. **Source Code Management:**  
   Connect the job to a source code repository like Git to pull the latest code.

3. **Build Triggers:**  
   Configure when the job should run (e.g., on code push, schedule).

4. **Build Steps:**  
   Specify tasks such as Execute Shell
   ```shell
   echo "Avesh Here learning Jenkins"

   echo "Welcomes Here"

   mkdir -p devops

   echo "Devops folder Created"

   ```

5. **Post-build Actions:**  
   Actions after build, like emailing notifications or archiving artifacts.

### For Scripted Pipeline Example
1. **Create a New pipeline Job:**
   Define the job name and type (Pipeline)
> NOTE: All the Things are same but here we give a scripted pipeline written in groovy language in build steps.

2. **Build Steps:**
   Specify task Like Stepes and stages
   ```groovy
   pipeline {
    agent Any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello Friends'
            }
        }
        stage('devops') {
            steps {
                sh "mkdir -p devops"
            }
        }
        stage('build') {
            steps {
                echo "Bye Friends"
            }
        }
     }
   }
   ```
---

# 🖥️ Setting Up Jenkins Agent Notes 

## Overview
A Jenkins agent (also called a node or slave) is a machine that executes jobs delegated by the Jenkins controller (master). Agents help distribute workloads, scale builds, and isolate environments.

## ✅ Prerequisites 
- Java (JDK 8 or newer) must be installed. in Agent Node

## Steps to Set Up an Agent using SSh

**In Master**
Step 1: go to `cd ~/.ssh` and do `ls`
Step 2: generate a SSH key (This will make a private key as well as Public key) and put all the things empty
```bash
ssh-keygen
```
Step 3: if we check Now we have a private key as well as Public key. `id_ed25519` this is private key `id_ed25519.pub` this is public key and Copy Both contents
```bash
cat id_ed25519
cat id_ed25519.pub 
```

**In Agent**
Step 4: Install JDk
```
sudo apt update
sudo apt install fontconfig openjdk-21-jre
```
Step 5: got to `cd ~/.ssh` and do `ls`
Step 6: paste the Public Key `id_ed25519.pub` inside the file authorized_keys
```bash
sudo vim authorized_keys
```
**In Jenkins UI**
Step 7: Go to Manage Jenkins -> Nodes -> New Node
Step 8: Give The name and select Parmanent Agent and Next
Step 9: keep All the things Same only Chnage below Things
```
Remote root directory = /home/azureuser       # this can change if you use aws or any other
labels = build          # you can use anything
Launch method = Luanch Agent via SSH
```
Step 10: Add the SSh Key in Jenkins Ui for Connection 
```
Host = azureuser             # you can check your host and Write the Specific host
confidential -> + Add
  kind =  ssh username and private key
  id = ubuntu-agent-key
  username = azureuser # tick the enter Directly
  PrivateKey = Add private key `id_ed25519` in Enter Directly Box of Private key 
Host Key Verification Strategy = non verifying verification Strategy
```
Step 11: Now Save and Click on Luanch Agent
---
