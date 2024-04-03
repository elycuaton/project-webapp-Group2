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
                        sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 992382806869.dkr.ecr.us-east-1.amazonaws.com'
                        def appImage = docker.build("$ECR_REGISTRY:${env.BUILD_ID}")
                        appImage.push()
                    } 
                }
            }
        }
    }
}
