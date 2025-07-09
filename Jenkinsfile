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
    stage{
      name: 'Deploy to Nexus'
      steps {
        nexusArtifactUploader artifacts: [[artifactId: 'Spring-Clinic', classifier: '', file: 'target/spring-petclinic-3.1.0-SNAPSHOT.jar', type: '.jar']], credentialsId: 'Spring-Clinic', groupId: 'org.springframework.samples', nexusUrl: 'localhost:8082', nexusVersion: 'nexus3', protocol: 'http', repository: 'Spring-Clinic', version: '3.1.0'
        }
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