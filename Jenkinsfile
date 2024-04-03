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

        stage('Build and Push Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'aws-ecr-credentials', passwordVariable: 'AWS_SECRET', usernameVariable: 'AWS_ID')]) {
                        sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/k1x3p9a5'
                        def appImage = docker.build("$ECR_REGISTRY:${env.BUILD_ID}")
                        appImage.push()
                    } 
                }
            }
        }
    }
}
