pipeline {
    
    agent any
    
    stages{
        stage("Clone The code..."){
            steps{
                echo "Cloning the code"
                git url: "https://github.com/Amitabh-DevOps/Django-notes-app-declarative-cidd.git", branch: "main"
            }
        }
        stage("Build and Test..."){
            steps{
                echo "Building the Docker image(Container)"
                sh "docker build . -t cicd-note-app:latest"
            }
        }
        stage("Push build to Docker Hub"){
            steps{
                echo "Pushing build to DockerHub..."
                withCredentials([
                    usernamePassword(
                        credentialsId: "dockerHub",
                        passwordVariable: "dockerHubPass",
                        usernameVariable: "dockerHubUser"
                    )    
                ]){
                    sh "docker tag cicd-note-app ${env.dockerHubUser}/cicd-note-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/cicd-note-app:latest"
                }
            }
        }
        stage("Deploy the Container"){
            steps{
                echo "Deploying docker Container..."
                sh "docker compose down && docker compose up -d"
                
            }
        }
    }
}
