pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps {
              echo "Clonning the code"  
              git url:"https://github.com/LondheShubham153/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps {
                echo "Building Docker image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to DockerHub"){
            steps {
                echo "pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker tag my-note-app ${env.dockerhubuser}/my-note-app:latest"
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker push ${env.dockerhubuser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "deplying to EC2"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
