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
                checkout scm
            }
        }

        stage('Login to ECR Public') {
            steps {
                script {
                    // Log in to AWS ECR Public non-interactively
                    sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/k1x3p9a5'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t group2-repository .'
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    sh 'docker tag group2-repository:latest public.ecr.aws/k1x3p9a5/group2-repository:latest'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker push public.ecr.aws/k1x3p9a5/group2-repository:latest'
                }
            }
        }
    }
}
