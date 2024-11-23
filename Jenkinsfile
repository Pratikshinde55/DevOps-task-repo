pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials-id') // Docker Hub credentials stored in Jenkins
    }
    stages {
        stage('Clone Repository') {
            steps {
                // Cloning the GitHub repository
                git branch: 'main', url: 'https://github.com/<your-username>/DevOps-App.git'
            }
        }
        stage('Build Application') {
            steps {
                // If required, add commands to build Java/Python/PHP application
                script {
                    echo "Building the application..."
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                script {
                    docker.build('devops-app')
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                // Push the Docker image to Docker Hub
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        docker.image('devops-app').push("latest")
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}

