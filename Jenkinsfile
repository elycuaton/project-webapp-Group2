pipeline {
    agent any

    environment {
        // Define your public ECR repository
        ECR_REGISTRY = 'public.ecr.aws/k1x3p9a5/group2-repository'
        // Define your AWS region
        AWS_REGION = 'us-east-1'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the source code from SCM (e.g., Git)
                checkout scm
            }
        }

        stage('Build and Push Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'aws-ecr-credentials', passwordVariable: 'AWS_SECRET', usernameVariable: 'AWS_ID')]) {
                        // Login to the AWS public ECR
                        sh 'aws ecr-public get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}'

                        // Build the Docker image using the Dockerfile in your project
                        def appImage = docker.build("${ECR_REGISTRY}:${env.BUILD_ID}")

                        // Push the built image to your AWS public ECR repository
                        appImage.push()
                    }
                }
            }
        }
    }
}
