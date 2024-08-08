pipeline{
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('clone repo') {
            script {
                checkout scm
            }
        }
        stage('Build Image') {
            app = docker.build("ubuntunginx")
        }
        stage('Push to ECR') {
            script {
                docker.withRegistry('https://438894829072.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:visualpathdemo01') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                }
            }
        }
    }
}