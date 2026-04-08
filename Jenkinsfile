pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/dev9234-gif/devops-cicd-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t node-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop node-app || true
                docker rm node-app || true
                docker run -d -p 3000:3000 --name node-app node-app
                '''
            }
        }
    }
}
