pipeline {
    agent any
    stages {
        stage("Code"){
            steps{
                git url: 'https://github.com/ShreyasSahoo/node-todo-cicd.git', branch: 'master'
            }
            
        }
        stage("Build & Test" ){
            steps{
                sh "docker build . -t node-app-test-new"
                
            } 
        }
        stage("Push to DockerHub"){
             steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubusername")]){
                    sh "docker login -u ${env.dockerhubusername} -p ${env.dockerhubpass}"
                    sh "docker tag node-app:latest ${env.dockerhubusername}/node-app-test-new:latest"
                    sh "docker push ${env.dockerhubusername}/node-app-test-new:latest"
                }
             }
        }
        stage("Deploy"){
             steps{
                 sh "docker image prune"
                 sh "docker-compose down && docker-compose up -d"
                 
             }
        }
    }
}
