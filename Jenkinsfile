pipeline {
    agent any

    environment {
        ECR_PUBLIC_REGISTRY = 'public.ecr.aws/your-registry-alias' // Replace with your actual public registry alias
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'aws-credentials', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/k1x3p9a5'
                    }
                }
            }
        }

        stage('Build and Push Image') {
            steps {
                script {
                    def appImage = docker.build("${ECR_PUBLIC_REGISTRY}/group2-repository:${env.BUILD_ID}") 
                    appImage.push()
                }
            }
        }
    }
}
