pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "skillfullsky/flask-app:latest"
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'DOCKERHUB_CREDENTIALS', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    }
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    sh 'docker push ${DOCKER_IMAGE}'
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh 'docker rmi ${DOCKER_IMAGE} || true'
                    sh 'docker logout'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and Push Successful!'
        }
        failure {
            echo '❌ Build or Push Failed!'
        }
    }
}
