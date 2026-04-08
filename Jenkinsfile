pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        ECR_REPO = "117911340225.dkr.ecr.us-east-1.amazonaws.com/devops/cicd"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/dev9234-gif/devops-cicd-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t node-app .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag node-app:latest $ECR_REPO:latest'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin $ECR_REPO
                '''
            }
        }

        stage('Push to ECR') {
            steps {
                sh 'docker push $ECR_REPO:latest'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop node-app || true
                docker rm node-app || true
                docker run -d -p 3000:3000 --name node-app $ECR_REPO:latest
                '''
            }
        }
    }
}
