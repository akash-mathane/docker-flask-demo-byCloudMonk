pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials') // Jenkins credentials ID
        IMAGE_NAME = "skillfullsky/flask-app"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/your-github-username/your-repo.git'  // Replace with your actual GitHub repo URL
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    sh """
                    echo '${DOCKER_HUB_CREDENTIALS_PSW}' | docker login -u '${DOCKER_HUB_CREDENTIALS_USR}' --password-stdin
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                    docker build -t ${IMAGE_NAME}:${DOCKER_TAG} .
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh """
                    docker push ${IMAGE_NAME}:${DOCKER_TAG}
                    """
                }
            }
        }

        stage('Logout from Docker Hub') {
            steps {
                sh "docker logout"
            }
        }
    }

    post {
        always {
            sh "docker system prune -f"
        }
    }
}
