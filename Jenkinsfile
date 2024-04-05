pipeline {
    environment {
        // Define the ECR registry URL
        ECR_REGISTRY = 'public.ecr.aws/k1x3p9a5'
        // Define your repository name
        ECR_REPOSITORY = 'group2-repository'
        // Define your image tag, could be dynamic based on Git commit or a static value like 'latest'
        IMAGE_TAG = 'latest'
    }
    
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Checks out the Git repository
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                // Builds the Docker image and tags it with the ECR repository
                script {
                    docker.build("${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}")
                }
            }
        }

        stage('Login to AWS ECR') {
            steps {
                script {
                    // Login to AWS ECR. You need to have AWS CLI installed and configured on Jenkins
                    sh "aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Pushes the Docker image to ECR
                    sh "docker push ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
                }
            }
        }

        // Optional: Additional stages to deploy your Docker image to AWS ECS or other services
        
        stage('Deploy to AWS') {
            steps {
                script {
                    // Your deployment script goes here. This might involve updating an ECS service or a Kubernetes deployment
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images to ensure the Jenkins agent doesn't run out of disk space
            script {
                sh "docker rmi ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
            }
        }
    }
}
