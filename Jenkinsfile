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
                sh "docker tag mynote-app ${env.dockerHubUser}/mynote-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/mynote-app:latest"
                }
            }
            
        }
        stage("Deploy"){
            steps {
                 echo "Deploying the container."
                 sh "docker-compose down && docker-compose up -d"
            }
            
        }
    }
}
