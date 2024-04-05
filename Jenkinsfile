pipeline {
    agent any

    environment {
        // Define your public ECR repository URI
        ECR_REGISTRY = 'public.ecr.aws/k1x3p9a5/group2-repository'
        // Define the AWS region your ECR repository resides in
        AWS_REGION = 'us-east-1'
        // Define the Docker image name
        IMAGE_NAME = 'demo'
    }

    stages {
        stage('Pre-Checks') {
            steps {
                script {
                    // Check Docker & AWS CLI version
                    sh 'docker --version'
                    sh 'aws --version'
                }
            }
        }

        stage('Checkout Code') {
            steps {
                // Checkout the source code from SCM (e.g., Git)
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    // Build the Docker image with the 'demo' tag
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'aws-ecr-credentials', passwordVariable: 'AWS_SECRET', usernameVariable: 'AWS_ID')]) {
                        // Log in to AWS ECR
                        sh 'aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}'

                        // Tag and push the 'latest' tag to ECR
                        docker.image("${IMAGE_NAME}").push('latest')
                    }
                }
            }
        }
    }
}
