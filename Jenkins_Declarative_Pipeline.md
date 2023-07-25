# Developing Jenkins Declarative pipeline.

These are 4 stages are are going to use while buildig the pipeline.

| Clone the Git Code  | Build docker image | Deploy image to DockerHub | Launch the container
| ------------- | -------------- | ------------- | -------------- |


### [1]. Installation of Jenkins.
```
#yum install java-11-openjdk -y
#curl --silent --location http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo | sudo tee /etc/yum.repos.d/jenkins.repo
#sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
#sudo yum install jenkins
#systemctl start jenkins
#systemctl enable jenkins
```

### [2]. Deploy the pipeline with following Groovy script.
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
}
```


Few pointes to note:
1. We are using the application from https://github.com/LondheShubham153/django-notes-app/tree/main.
2. Hide the credential with the envrionment property in jenkins.
3. Tried the jenkins wth **Pipeline with Script** and **Pipeline script from SCM** both the options.

![Alt text](image link)
