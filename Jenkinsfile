pipeline {
    agent any

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

        stage('Login to ECR Public') {
            steps {
                script {
                    // Log in to AWS ECR Public
                    sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/k1x3p9a5'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t group2-repository .'
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    // Tag the Docker image
                    sh 'docker tag group2-repository:latest public.ecr.aws/k1x3p9a5/group2-repository:latest'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to AWS ECR Public
                    sh 'docker push public.ecr.aws/k1x3p9a5/group2-repository:latest'
                }
            }
        }
    }
}
