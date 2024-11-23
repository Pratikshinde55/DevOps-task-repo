pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials-id') // Replace with your Docker Hub credentials ID
        GIT_REPO_URL = 'https://github.com/Pratikshinde55/DevOps-task-repo.git' // Your GitHub repo
        DOCKER_IMAGE_NAME = 'pratikshinde55/devops-app' // Your Docker Hub image name
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning GitHub repository...'
                git url: "${GIT_REPO_URL}", branch: 'main' 
            }
        }

        stage('Build Application') {
            steps {
                echo 'Building the application...'
                // Add commands to build your application based on its type (Java, Python, PHP)
                // Example for Java:
                sh 'mvn clean package' // For Python or PHP, use the appropriate build commands
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh '''
                docker build -t ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo 'Pushing Docker image to Docker Hub...'
                sh '''
                echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                docker push ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying the application...'
                sh '''
                docker stop devops-app || true
                docker rm devops-app || true
                docker run -d --name devops-app -p 8080:8080 ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}

