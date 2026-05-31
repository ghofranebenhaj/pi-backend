pipeline {
    agent any

    environment {
        JAVA_HOME = '/opt/java/openjdk'
        MAVEN_HOME = '/usr/share/maven'
        PATH = "${MAVEN_HOME}/bin:${JAVA_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/jogamingG5/pi-backend.git',
                    branch: 'main'
            }
        }

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
                    docker run -d --name pi-backend -p 8081:8081 --restart unless-stopped pi-backend:latest
                '''
            }
        }
    }

    post {
        success {
            echo "========================================="
            echo "📧 EMAIL SIMULATION - NOTIFICATION"
            echo "========================================="
            echo "✅ Statut: SUCCESS"
            echo "📁 Projet: ${env.JOB_NAME}"
            echo "🔢 Build: #${env.BUILD_NUMBER}"
            echo "🔗 URL: ${env.BUILD_URL}"
            echo "📧 Destinataire: youssef.zaiene.yz@gmail.com"
            echo "========================================="
        }
        failure {
            echo "========================================="
            echo "📧 EMAIL SIMULATION - NOTIFICATION"
            echo "========================================="
            echo "❌ Statut: FAILED"
            echo "📁 Projet: ${env.JOB_NAME}"
            echo "🔢 Build: #${env.BUILD_NUMBER}"
            echo "🔗 URL: ${env.BUILD_URL}"
            echo "📧 Destinataire: youssef.zaiene.yz@gmail.com"
            echo "========================================="
        }
    }
}
