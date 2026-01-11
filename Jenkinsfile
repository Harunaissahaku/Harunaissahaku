
pipeline {
    agent any

    tools {
        maven 'maven3.9.11' // Use your predefined Maven installation
    }

    environment {
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_TOKEN = credentials('SonarQubeToken') // Jenkins secret token
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out code..."
                git branch: 'main', url: 'https://github.com/Harunaissahaku/jomacs_web-apps.git'
            }
        }

        stage('Build & Unit Test') {
            steps {
                echo "Building the project with Maven..."
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube scanner..."
                withSonarQubeEnv('SonarQube') { // Must match SonarQube installation in Jenkins
                    sh "mvn sonar:sonar -Dsonar.projectKey=my-web-app -Dsonar.projectName=my-web-app -Dsonar.host.url=${SONAR_HOST_URL} -Dsonar.token=${SONAR_TOKEN}"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo "Waiting for SonarQube Quality Gate..."
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
