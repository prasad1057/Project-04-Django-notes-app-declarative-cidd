pipeline {
    
    agent any
    
    stages {
        stage("Clone Code") {
            steps {
                git url: "https://github.com/Amitabh-DevOps/Django-notes-app-declarative-cidd.git", branch: "main"
                echo "Aaj toh LinkedIn Post bannta hai boss"
            }
        }
        stage("Build & Test") {
            steps {
                sh "docker build . -t todo-note-app:latest"
            }
        }
        stage("Push to DockerHub") {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: "dockerHub",
                        passwordVariable: "dockerHubPass", 
                        usernameVariable: "dockerHubUser"
                    )
                ]) {
                    sh "docker tag todo-note-app ${env.dockerHubUser}/todo-note-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/todo-note-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying docker container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
