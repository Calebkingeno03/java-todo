pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'eu-west-1' // Replace with your AWS region
        ECR_REPOSITORY = '058264506809.dkr.ecr.eu-west-1.amazonaws.com/moringa-repo'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Build the Docker image
                sh 'docker build -t moringa-repo .'
            }
        }
        
        stage('Push to ECR') {
            steps {
                // Get ECR login password and login to ECR
                withCredentials([string(credentialsId: 'your-aws-ecr-credentials', variable: 'DOCKER_LOGIN_PASSWORD')]) {
                    sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${ECR_REPOSITORY}"
                }
                
                // Tag the Docker image
                sh "docker tag moringa-repo:latest ${ECR_REPOSITORY}:latest"
                
                // Push the Docker image to ECR
                sh "docker push ${ECR_REPOSITORY}:latest"
            }
        }
    }
}

