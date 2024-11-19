pipeline{
    agent {label "dev"}
    stages{
        stage("Code Clone"){
            steps{
                echo "Code Clone Stage"
                git url: "https://github.com/Vishal-Kumar-CSE/node-todo-cicd.git", branch: "master"
            }
        }

        stage('Image Build'){
            steps{
                sh "docker build --no-cache -t node-app ."
            }
        }
        stage("Push to Dockerhub"){
            steps{
                withCredentials([usernamePassword(
                credentialsId:"dockerHubCreds",
                usernameVariable:"dockerHubUser",
                passwordVariable: "dockerHubPass")]) {
    // Your code here
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag node-app:latest ${env.dockerHubUser}/node-app"            
                    sh "docker push ${env.dockerHubUser}/node-app:latest"
                }
            }
        }
        stage('Deploy'){
            steps{
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}
