pipeline {
    agent any
    
    stages {
        stage("Code") {
            steps {
                echo "Cloning the code"  
                git url: "https://github.com/rehanabbasi786/django-notes-app.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Building Docker image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to DockerHub") {
            steps {
                echo "Pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: "dockerhubpass", usernameVariable: "dockerhubuser")]) {
                    sh "docker tag my-note-app ${env.dockerhubuser}/my-note-app:latest"
                    sh "echo ${env.dockerhubpass} | docker login -u ${env.dockerhubuser} --password-stdin"
                    sh "docker push ${env.dockerhubuser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying to EC2"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
