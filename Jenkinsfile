pipeline{
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('clone repo') {
            steps {
                script {
                    checkout scm
                }
            }
        }
        stage('Build Image') {
            steps {
                script {
                    app = docker.build("ubuntunginx:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push to ECR') {
            steps {
                script {
                        docker.withRegistry('https://438894829072.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:visualpathdemo01') {
                    app.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }
        stage('Scan Image') {
            steps {
                sh "trivy image 438894829072.dkr.ecr.us-east-1.amazonaws.com/ubuntunginx:${env.BUILD_NUMBER}"
            }
        }
    }
}