pipeline {
    agent any

    environment {
        // Define environment variables here if needed
        COMPOSE_PROJECT_NAME = 'school-management'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checks out the source code from your Git repository
                checkout scm
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    // Using sh for Linux/macOS Jenkins agents. 
                    // Note: If your Jenkins runs directly on Windows, change 'sh' to 'bat'
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'npm install'
                    // Uncomment the line below once you add tests to backend/package.json
                    // sh 'npm test' 
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                // Validates that the Dockerfiles build successfully
                sh 'docker compose build'
            }
        }

        stage('Deploy') {
            steps {
                // Starts the application in detached mode (-d)
                sh 'docker compose up -d'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution finished.'
        }
        success {
            echo '✅ Application successfully built and deployed!'
        }
        failure {
            echo '❌ Pipeline failed. Please check the logs.'
            // Optional: stop containers if the deployment fails
            // sh 'docker compose down'
        }
    }
}
