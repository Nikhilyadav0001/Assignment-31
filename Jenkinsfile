pipeline {
    agent any

    environment {
        BACKEND_DIR  = 'backend'
        FRONTEND_DIR = 'frontend'
    }

    stages {
        stage('Checkout') {
            steps {
                // Wipe workspace before fresh clone
                deleteDir()
                checkout scm
            }
        }

        stage('Backend Install & Test') {
            steps {
                dir(env.BACKEND_DIR) {
                    script {
                        if (fileExists('package.json')) {
                            bat 'npm install'
                        }
                    }
                }
            }
        }

        stage('Frontend Install & Build') {
            steps {
                dir(env.FRONTEND_DIR) {
                    script {
                        if (fileExists('package.json')) {
                            bat 'npm install'
                            bat 'npm run build'
                        }
                    }
                }
            }
        }

        stage('Docker Build & Compose') {
            steps {
                script {
                    if (fileExists('docker-compose.yml')) {
                        bat 'docker compose down --remove-orphans'
                        bat 'docker rm -f schooldb school_backend school_frontend || exit 0'
                        bat 'docker compose up -d --build'
                    }
                }
            }
        }
    }

    post {
        always {
            // cleanWs() needs an agent/node context — wrap it
            node('') {
                cleanWs()
            }
        }
    }
}