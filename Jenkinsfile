pipeline{
    agent any

    environment{
        DOCKER_IMAGE = "my-react-app:latest"

    }
    stages{
        stage('checkout'){
            steps{
                echo 'pulling the code  ...'
                checkout scm
            }

        }
        stage('install & Build'){
            steps{
                 echo '⚙️ Installing dependencies and building React app...'
                bat 'npm install'
                bat 'npm run build'
            }
        }
        stage ('docker Build'){
            steps{
                   echo '🐳 Building Docker image ...'
                   bat "docker build -t ${DOCKER_IMAGE}."
            }
        }
        stage('Install & Build') {
            steps {
                echo '⚙️ Installing dependencies and building React app...'
                bat 'npm install'
                bat 'npm run build'
            }
        
        }
        stage('Docker Run') {
            steps {
                echo '🚀 Running container...'
                bat "docker run -d -p 3000:80 --name my-react-container ${DOCKER_IMAGE}"
            }
        }
    }
}