pipeline {
    agent any 
    environment {
        DOCKERHUB_CREDENTIALS = credentials('skillfullsky')
    }
    stages { 
        stage('Build docker image') {
            steps {  
                sh 'docker build -t skillfullsky/flaskapp:$BUILD_NUMBER .'
            }
        }
        
        stage('Login to DockerHub') {
            steps{
                sh 'echo "$DOCKERHUB_CREDENTIALS_PSW" | docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin'
            }
        }
        
        stage('Push Image') {
            steps{
                sh 'docker push skillfullsky/flaskapp:$BUILD_NUMBER'
            }
        }
    }

    post {
        always {
            script {
                node {  // Ensure workspace context is available
                    sh 'docker logout'
                }
            }
        }
    }
}
