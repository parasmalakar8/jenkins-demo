pipeline {
    agent any
    environment {
        AWS_REGION = "ap-south-1"
        ECR_REGISTRY = "967170426743.dkr.ecr.ap-south-1.amazonaws.com"
        ECR_REPO = "my-third-project"
        IMAGE_TAG = "latest"
    }
    stages {
        stage('Login to ECR') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-s3-credentials'
                ]]) {
                    bat "aws ecr get-login-password --region %AWS_REGION% | docker login --username AWS --password-stdin %ECR_REGISTRY%"
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                bat "docker build -t %ECR_REGISTRY%/%ECR_REPO%:%IMAGE_TAG% ."
            }
        }
        stage('Push to ECR') {
            steps {
                bat "docker push %ECR_REGISTRY%/%ECR_REPO%:%IMAGE_TAG%"
            }
        }
    }
}
