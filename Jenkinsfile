pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/ShashankShekhar23/node_demo_app.git", branch: "master"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t node-app-jenkins"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"DockerHub",passwordVariable:"DockerHubPass",usernameVariable:"DockerHubUser")]){
                    sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPass}"
                    sh "docker tag node-app-jenkins ${env.DockerHubUser}/node-app-jenkins:latest"
                    sh "docker push ${env.DockerHubUser}/node-app-jenkins:latest" 
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
