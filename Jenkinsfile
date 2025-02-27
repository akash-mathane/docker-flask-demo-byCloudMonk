pipeline {
    agent any 
    environment {
        DOCKERHUB_CREDENTIALS = credentials('skillfullsky')
    }
    stages { 
        stage('Build docker image') {
            steps {  
                sh '''
                set -x
                docker build -t skillfullsky/flask-app:$BUILD_NUMBER .
                '''
            }
        }
        
        stage('Login to DockerHub') {
            steps{
                sh '''
                set -x
                echo "$DOCKERHUB_CREDENTIALS_PSW" | docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin
                '''
            }
        }
        
        stage('Push Image') {
            steps{
                sh '''
                set -x
                docker push skillfullsky/flaskapp:$BUILD_NUMBER
                '''
            }
        }
    }

    post {
        always {
            script {
                node {
                    sh '''
                    set -x
                    docker logout || echo "Docker logout failed, but continuing..."
                    '''
                }
            }
        }
    }
}
