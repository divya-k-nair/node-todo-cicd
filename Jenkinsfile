pipeline{
    agent { label 'dev-server' }
    stages{
        stage("Code Clone"){
            steps{
                echo "code clone stage"
                git url: "https://github.com/divya-k-nair/node-todo-cicd", branch: "master"
                
            }
        }
        stage("Code Build and Test"){
            steps{
                 echo "code Build and Test stage"
                sh "docker build -t node-app ."
            }
        }
        stage("Push To DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    usernameVariable:"dockerHubUser",
                    passwordVariable:"dockerHubPass")]){
                 sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                sh "docker image tag node-app:latest ${env.dockerHubUser}/node-app:latest"
                sh "docker push ${env.dockerHubUser}/node-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
