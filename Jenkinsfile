pipeline {
    agent any

    stages {
        stage('Pre-Checks') {
            steps {
                script {
                    // Check Docker & AWS CLI version
                    bat 'docker --version'
                    bat '"C:\\Program Files\\Amazon\\AWSCLIV2\\aws.exe" --version'
                }
            }
        }

        stage('Check Environment on Jenkins Agent') {
            steps {
                script {
                    // Print PATH environment variable to check if AWS CLI v2 path is included
                    bat 'echo %PATH%'
                }
            }
        }

        stage('Login to ECR Public') {
            steps {
                script {
                    // Log in to AWS ECR Public using AWS CLI v2
                    bat '"C:\\Program Files\\Amazon\\AWSCLIV2\\aws.exe" ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/k1x3p9a5'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    bat 'docker build -t group2-repository .'
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    // Tag the Docker image
                    bat 'docker tag group2-repository:latest public.ecr.aws/k1x3p9a5/group2-repository:latest'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to AWS ECR Public
                    bat 'docker push public.ecr.aws/k1x3p9a5/group2-repository:latest'
                }
            }
        }
    }
}
