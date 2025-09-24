pipeline {
    agent any

    tools {
        nodejs "nodejs"   // Make sure NodeJS is configured in Jenkins → Global Tool Config
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Asfand543/react.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo "📦 Installing dependencies..."
                    bat "npm install"
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "⚡ Building React app..."
                    // Ensure index.html exists before build
                    bat '''
                    if not exist index.html (
                        echo ERROR: index.html missing in project root!
                        exit 1
                    )
                    npm run build
                    '''
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "✅ Build completed successfully!"
        }
        failure {
            echo "❌ Build failed. Check logs!"
        }
    }
}
