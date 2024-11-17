pipeline {
    agent any
    
    environment {
        AWS_REGION = 'ap-southeast-2'
        ECR_REPO = 'group18/devops_project'
        URL_REGISTRY = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
    }
    
    stages { 
        stage('SCM Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'ecr-demo-credential', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        // Login to ECR
                        sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${URL_REGISTRY}"

                        // Build Docker image
                        sh "docker build -t $ECR_REPO ."

                        // Tag Docker image
                        sh "docker tag $ECR_REPO:latest ${URL_REGISTRY}/$ECR_REPO:latest"

                        // Push Docker image to ECR
                        sh "docker push ${URL_REGISTRY}/$ECR_REPO:latest"
                    }
                }
            }
        }
    }
}
