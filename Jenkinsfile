pipeline {
    environment {
        ECR_REGION = 'us-east-2'
        ECR_REGISTRY = '992382763363.dkr.ecr.us-east-2.amazonaws.com'
        ECR_REPOSITORY = 'group2-repository'
        IMAGE_TAG = 'latest'
    }
    
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build("${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}")
                }
            }
        }

        stage('Login to AWS ECR') {
            steps {
                script {
                    sh 'aws ecr-public get-login-password --region ${ECR_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
                }
            }
        }
    }
}


