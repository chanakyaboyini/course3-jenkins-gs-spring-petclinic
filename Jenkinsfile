pipeline {
  agent any

  tools {
    maven 'maven3'    // your Jenkins Maven tool name
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'mvn compile'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }

    stage('Publish to Nexus') {
      steps {
        sh 'mvn package'
      }
    }
  }

  post {
    success {
      echo "✅ Build, test, and package completed successfully."
    }
    failure {
      echo "❌ Pipeline failed – check the console logs."
    }
  }
}