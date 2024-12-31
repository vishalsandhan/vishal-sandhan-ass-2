pipeline {
    agent any
    tools {
        nodejs 'nodejs-16.20.2'
    }
    environment {
        NODEJS_HOME = 'C:/Program Files/nodejs' 
        // SONARQUBE = 'SonarQube'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm install
                '''
            }
        }
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token') // Accessing the SonarQube token stored in Jenkins credentials
            }
            steps {
                // Ensure that sonar-scanner is in the PATH
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                where sonar-scanner || echo "SonarQube scanner not found. Please install it."
                sonar-scanner -Dsonar.projectKey=sonar-web ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=http://localhost:9000 ^
                    -Dsonar.token=%SONAR_TOKEN% 2>&1
                '''
            }
        }
    }
  post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
