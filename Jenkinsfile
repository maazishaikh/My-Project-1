pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"  // change to your AWS region
        ECR_REPO = "972583819253.dkr.ecr.ap-south-1.amazonaws.com/my-project-1"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                git branch: 'master', url: 'https://github.com/maazishaikh/My-Project-1'
            }
        }

        stage('Login to AWS ECR') {
            steps {
                script {
                    sh '''
                    aws --version
                    aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPO
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                    docker build -t $ECR_REPO:$IMAGE_TAG .
                    '''
                }
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                script {
                    sh '''
                    docker push $ECR_REPO:$IMAGE_TAG
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Docker Image successfully pushed to ECR: $ECR_REPO:$IMAGE_TAG"
        }
        failure {
            echo "❌ Build failed. Check logs."
        }
    }
}
