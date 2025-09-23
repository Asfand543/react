pipeline {
  agent any

  environment {
    NODE_ENV = 'production'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm ci'
      }
    }

    stage('Build React App') {
      steps {
        sh 'npm run build'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t my-react-app .'
      }
    }

    stage('Docker Run') {
      steps {
        sh 'docker run -d -p 3000:3000 my-react-app'
      }
    }
  }

  post {
    success {
      echo '✅ Build and deployment succeeded!'
    }
    failure {
      echo '❌ Build failed. Check logs for details.'
    }
  }
}