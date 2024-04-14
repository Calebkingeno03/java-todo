pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'eu-west-1' // Replace with your AWS region
        AWS_ACCESS_KEY_ID = credentials('AKIAQ3EGV2G4QSLT6L7U')
        AWS_SECRET_ACCESS_KEY = credentials('fwOHm/5tzNr/pJptqER/oNXivD3PBmbeAOlonznX')
        ECR_REPO = 'moringa-repo'
        IMAGE_TAG = 'latest'
    }
    
    stages {
        stage('Build') {
            steps {
                script {
                    // Login to Amazon ECR
                    withCredentials([string(credentialsId: 'eu-west-1:ecr_credentials', variable: 'DOCKER_LOGIN')]) {
                        sh "docker login -u AWS -p ${DOCKER_LOGIN} https://${AWS_DEFAULT_REGION}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                    }
                    
                    // Build Docker image
                    sh 'docker build -t ${ECR_REPO}:${IMAGE_TAG} .'
                }
            }
        }
        
        stage('Push') {
            steps {
                script {
                    // Push Docker image to Amazon ECR
                    sh "docker push ${ECR_REPO}:${IMAGE_TAG}"
                }
            }
        }
    }
}
