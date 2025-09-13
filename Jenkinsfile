pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"  // change to your AWS region
        ECR_REPO = "your-account-id.dkr.ecr.ap-south-1.amazonaws.com/your-repo-name"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-repo.git'
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
