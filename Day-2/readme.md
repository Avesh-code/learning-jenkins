# Jenkins Shared Libraries 📚

## What are Shared Libraries? 🤔
- Reusable Groovy code libraries for Jenkins pipelines.
- Stored in separate Git/SCM repositories.
- Share common pipeline steps, functions, and classes across multiple Jenkins jobs.

## Key Structure 🏗️
- `vars/` directory: global variables & simple functions (used like pipeline steps).
- `src/` directory: reusable Groovy classes & more complex logic.
- Versioned via Git branches or tags for safe updates.

## Benefits 🌟
- Avoids code duplication.
- Centralized updates for pipeline logic.
- Cleaner, modular Jenkinsfiles.
- Easier collaboration across teams.

## How to Use 🚀
- Configure in Jenkins global settings (Manage Jenkins → Configure System → Global Pipeline Libraries).
- Mention The Git Repo where the shared libraries written and Also configure the pipeline
- Created the functions Like `hello.groovy`:
  ```
  def call(){
  echo "Hello Welcome"
  }
  ```
- clone.groovy:
```
def call(String url, String branch){
  echo "This is Cloning the Code"
  git url: "${url}", branch: "${branch}"
  echo "Code Cloning is Successfull"
}
```
- docker_build.groovy:
```
def call(String projectname, String imagetag){
  sh """sudo docker build -t "${projectname}:${imagetag}" ."""
}
```
- docker_push.groovy:
```
def call(String projectname,String imagename){
  withCredentials([usernamePassword('credentialsId':"dockerHubCred",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")])
  {
    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass} "
    sh "docker image tag ${projectname}:${imagename} ${env.dockerHubUser}/${projectname}:${imagename} "
    sh "docker push ${env.dockerHubUser}/${projectname}:${imagename} "
  }
}
```

### In Pipeline
```groovy
@Library('Shared') _
pipeline{
    agent { label "main-agent" }

    stages{
        stage("hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        stage("Code"){
            steps{
                script{
                    clone("https://github.com/Avesh-code/django-notes-app.git","main")
                }
            }
        }
        stage("Build"){
            steps{
                script{
                      docker_build("notes-app","latest")
                }
            }
        }
        stage("push to docker hub"){
            steps{
                script{
                    docker_push("notes-app","latest")
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}
```
---
# Jenkins User Management 👥

## Overview
- Manage users who interact with Jenkins.
- Create, modify, and delete user accounts.
- Control user permissions and access levels.

## Authentication Methods 🔐
- Jenkins own user database (default for small setups).
- External identity providers: LDAP, Active Directory, GitHub, Google, etc.
- Delegate authentication to third-party systems for enterprise-grade security.

## Managing Users ⚙️
- Access via **Manage Jenkins > Manage Users**.
- Admins can create new users by providing username, password, full name, email.
- Modify user details or delete users as needed.

## Authorization & Roles 🎭
- Control what users can do in Jenkins using:
  - **Matrix-based security**: fine-grained permission control.
  - **Role-Based Authorization Strategy (RBAC)** plugin for assigning global and project-specific roles.
- Define global roles (admin, developer, viewer) and item roles (per-job access).
- Assign permissions like read, build, configure, or administer.
- In Practical Created a user and After that i have installed the plug-in Role-based access control strategy and After that i created a Role and Assign the role to that user.

---
