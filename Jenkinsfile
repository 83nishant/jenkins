pipeline{
    
    agent any;
    
    stages{
        stage("Code Clone"){
            steps{
               git url: "https://github.com/83nishant/jenkins.git", branch: "main"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app-2 ."
            }
            
        }
        stage("Test"){
            steps{
                echo "Developer / Tester..."
            }
            
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerhubcreds", 
                    passwordVariable: "dockerhubpass",
                    usernameVariable: "dockerhubuser"
                )]){
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker image tag two-tier-flask-app-2 ${env.dockerhubuser}/two-tier-flask-app-2"
                    sh "docker push ${env.dockerhubuser}/two-tier-flask-app-2:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose up -d --build flask-app-2"
            }
        }
    }
}
