pipeline {
    agent any

    tools {
        maven 'maven3.9.11'  // Make sure this matches your Jenkins Maven installation
    }

    environment {
        SONAR_HOST_URL = 'http://localhost:9000' // Your SonarQube server
    }

    stages {

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarScanner') {
                    sh 'mvn sonar:sonar -Dsonar.host.url=$SONAR_HOST_URL'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and SonarQube analysis successful'
        }
        failure {
            echo '❌ Build or SonarQube analysis failed'
        }
    }
}
