pipeline {
    agent any
    
    tools {
        nodejs 'nodejs'  // Configure this in Jenkins Global Tool Configuration
    }
    
    environment {
        DOCKER_IMAGE = 'react-app'
        DOCKER_TAG = "${env.BUILD_ID}"
    }
    
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
                bat 'echo "Repository checked out successfully"'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...'
                    bat 'npm install'
                }
            }
            post {
                success {
                    echo '✅ Dependencies installed successfully'
                }
                failure {
                    echo '❌ Dependency installation failed'
                }
            }
        }
        
        stage('Build React App') {
            steps {
                script {
                    echo 'Building React application...'
                    bat 'npm run build'
                }
            }
            post {
                success {
                    echo '✅ React app built successfully'
                }
                failure {
                    echo '❌ Build failed'
                }
            }
        }
        
        stage('Docker Build') {
            steps {
                script {
                    echo 'Building Docker image...'
                    bat "docker build -t ${env.DOCKER_IMAGE}:${env.DOCKER_TAG} ."
                }
            }
            post {
                success {
                    echo '✅ Docker image built successfully'
                }
                failure {
                    echo '❌ Docker build failed'
                    echo 'Make sure Docker is installed and running on Windows'
                }
            }
        }
        
        stage('Docker Run') {
            steps {
                script {
                    echo 'Running Docker container...'
                    bat 'docker stop react-container || true'
                    bat 'docker rm react-container || true'
                    bat "docker run -d -p 3000:3000 --name react-container ${env.DOCKER_IMAGE}:${env.DOCKER_TAG}"
                }
            }
            post {
                success {
                    echo '✅ Docker container running on port 3000'
                }
                failure {
                    echo '❌ Docker run failed'
                }
            }
        }
    }
    
    post {
        always {
            echo "Build ${currentBuild.result} - Pipeline execution completed"
            bat 'docker ps -a | findstr react-container'
        }
        success {
            echo '🎉 Pipeline executed successfully!'
            echo "Docker image: ${env.DOCKER_IMAGE}:${env.DOCKER_TAG}"
            echo 'Application should be available at http://localhost:3000'
        }
        failure {
            echo '💥 Pipeline failed. Check the logs above for details.'
            emailext (
                subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "Check console output at ${env.BUILD_URL}",
                to: "developer@company.com"
            )
        }
        unstable {
            echo '⚠️ Pipeline marked as unstable'
        }
        cleanup {
            bat 'echo "Cleaning up workspace..."'
            // Add any cleanup steps here
        }
    }
}