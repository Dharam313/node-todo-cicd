pipeline{
    agent {label "dev-server"}
    stages {
        stage("Clone code") {
            steps{
               git url: "https://github.com/Dharam313/node-todo-cicd.git", branch: "master" 
            }
        }
        stage("Build and Test") {
            steps{
               sh "docker build . -t node-app-test-new" 
            }
        }
        stage("Push to docker hub") {
            steps{
              withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
              sh "docker tag node-app-test-new ${env.dockerHubUser}/node-app-test-new:latest"
              sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
              sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
              }
            }
        }
        stage("Deploy") {
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
