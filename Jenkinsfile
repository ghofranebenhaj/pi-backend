pipeline {
    agent any

    stages {

        stage('Clean') {
            steps {
                sh 'mvn clean'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t pi-backend:latest .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop pi-backend || true
                    docker rm pi-backend || true
                    docker run -d \
                      --name pi-backend \
                      -p 8081:8081 \
                      --restart unless-stopped \
                      pi-backend:latest
                '''
            }
        }
    }

    post {
        success {
            echo "PIPELINE SUCCESS"
            echo "Projet: ${env.JOB_NAME}"
            echo "Build: #${env.BUILD_NUMBER}"
        }

        failure {
            echo "PIPELINE FAILED"
            echo "Projet: ${env.JOB_NAME}"
            echo "Build: #${env.BUILD_NUMBER}"
        }
    }
}
