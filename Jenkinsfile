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

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh '''
                    mvn sonar:sonar \\
                    -Dsonar.host.url=http://localhost:9000 \\
                    -Dsonar.login=squ_dc3a6030e7f58cf3455d964bc82b1de3a350e626
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t pi-backend:latest .'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline réussi !'
        }
        failure {
            echo '❌ Pipeline échoué !'
        }
    }
}
