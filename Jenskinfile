pipeline {
    agent any

    environment {
        ECR_REGISTRY = '992382806869.dkr.ecr.us-east-1.amazonaws.com/group2-repository'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                script {
                    def appImage = docker.build("${ECR_REGISTRY}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://992382806869.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:ecr-registry-credentials') {
                        def appImage = docker.image("${ECR_REGISTRY}:${env.BUILD_ID}")
                        appImage.push()
                    }
                }
            }
        }
    }
}

