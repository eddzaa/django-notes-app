pipeline {
    agent any
    stages {
        stage("Code") {
            steps {
                echo "clone the code"
                git url: "https://github.com/eddzaa/django-notes-app.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "build the image"
                sh "docker build -t notes-app ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "push the image to dockerhub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: "dockerhubPass", usernameVariable: "dockerhubUser")]) {
                    sh "docker tag notes-app ${dockerhubUser}/my-note-app:latest"
                    sh "docker login -u ${dockerhubUser} -p ${dockerhubPass}"
                    sh "docker push ${dockerhubUser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "deploy the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
