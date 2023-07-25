## Installation of jenkins.

#### You can install the jenkins on almost every platform, here is the steps to install the jenkins:
```
#yum install java-11-openjdk -y
#curl --silent --location http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo | sudo tee /etc/yum.repos.d/jenkins.repo
#sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
#sudo yum install jenkins
```

#### Start the jenkins process and make sure that you add the jenkins user to the docker group and restart the service.

```
#systemctl start jenkins
#systemctl enable jenkins
```
#### Pipeline details. 
This is simple pipeline. https://github.com/LondheShubham153/django-notes-app
```
pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps {
                echo "Cloning the code"
                git url: "https://github.com/LondheShubham153/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t mynote-app ."
            }
            
        }
        stage("Push to Docker"){
            steps {
                 echo "Pushing the image to docker."
                withCredentials([usernamePassword(credentialsId:"dockerhubcred", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) { 
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                }
            }
            
        }
        stage("Deploy"){
            steps {
                 echo "Deploying the container "    
            }
            
        }
    }
}```
