pipeline{
    agent any
    
    stages{
        stage("Clone"){
            steps {
                echo "Cloning code"
                git url:"https://github.com/arvindjai/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build"){
            steps{
                echo "Build docker images"
                sh "docker build . -t notes-app"
            }
        }
        stage("Push to DockerHUB"){
            steps{
                echo "Push to dockerhub"
                withCredentials([usernamePassword(credentialsId:"Dockerhub",usernameVariable: "dockerhubuser",passwordVariable: "dockerhubpass")]){
                sh "docker tag notes-app ${env.dockerhubuser}/notes-app:latest"
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker push ${env.dockerhubuser}/notes-app:latest"
                }
            }
            
        }
        stage("Deploy"){
            steps{
                echo "Deploying app"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
