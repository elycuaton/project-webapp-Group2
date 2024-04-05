pipeline {
    agent any

    environment {
        ECR_REGISTRY = 'public.ecr.aws/k1x3p9a5/group2-repository'
        AWS_REGION = 'us-east-1'
    }

    stages {
        stage('Pre-Checks') {
            steps {
                script {
                    // Check AWS CLI version
                    sh 'aws --version'
                }
            }
        }

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Build and Push Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'aws-ecr-credentials', passwordVariable: 'AWS_SECRET', usernameVariable: 'AWS_ID')]) {
                        // Use AWS CLI to log in to the ECR public repository
                        sh 'aws ecr-public get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin public.ecr.aws/k1x3p9a5'
                        
                        // Build and push Docker image
                        def appImage = docker.build("${ECR_REGISTRY}:${env.BUILD_ID}")
                        appImage.push()
                    }
                }
            }
        }
    }
}
