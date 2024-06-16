# Developing Jenkins Declarative pipeline.

These are 4 stages are are going to use while buildig the pipeline.

| Clone the Git Code  | Build docker image | Deploy image to DockerHub | Launch the container
| ------------- | -------------- | ------------- | -------------- |


### [1]. Installation of Jenkins.
```
#yum install java-11-openjdk -y
#sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
#sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
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
                sh "docker build -t my-note-app ."
            }
            
        }
        stage("Push to Docker"){
            steps {
                echo "Pushing the image to docker."
                withCredentials([usernamePassword(credentialsId:"dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) { 
                sh "docker tag my-note-app akshaysjadhav/my-note-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push akshaysjadhav/my-note-app:latest"
                }
            }
            
        }
        stage("Deploy"){
            steps {
                 echo "Deploying the container"
                 sh "docker run -d -p 8000:8000 akshaysjadhav/my-note-app:latest"
            }
            
        }
    }
}
```


Few pointes to note:
1. We are using the application from https://github.com/LondheShubham153/django-notes-app/tree/main.
2. Hide the credential with the envrionment property in jenkins.
3. Tried the jenkins wth **Pipeline with Script** and **Pipeline script from SCM** both options.


![ Pipeline with Script](https://github.com/AkshaySJadhav/DevOps/blob/main/resource/Pipeline_From_Script.png)
![ Pipeline with Script](https://github.com/AkshaySJadhav/DevOps/blob/main/resource/Pipeline_Script_From_SCM.png)


## Enabling Git Webhook.

To activate the pipeline when a code change is detected, follow these steps: Enable the webhook from the GitHub repository's Setting section and specify the URL of the Jenkins server.

Once you save the webhook configuration, make the change in the code and you will see new Job is getting launched.

Here is the configuration from my workstation: <br>
![ Pipeline with Script](https://github.com/AkshaySJadhav/DevOps/blob/main/resource/git_webhook.png)

