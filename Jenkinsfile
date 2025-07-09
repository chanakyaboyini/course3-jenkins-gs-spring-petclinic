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

  

    stage('Package') {
      steps {
        sh 'mvn package -DskipTests'
      }
    }

    stage('Deploy to Nexus') {
      steps {
        
        nexusArtifactUploader artifacts: [[artifactId: 'Spring-Clinic', classifier: '', file: 'target/spring-petclinic-3.1.0-SNAPSHOT.jar', type: '.jar']], credentialsId: 'Spring-Clinic', groupId: 'org.springframework.samples', nexusUrl: 'localhost:8082', nexusVersion: 'nexus3', protocol: 'http', repository: 'Spring-Clinic', version: '3.1.0'
          
        
      }
    }
  }

  post {
    success {
      echo '✅ Pipeline completed successfully.'
    }
    failure {
      echo '❌ Pipeline failed – check the console logs.'
    }
  }
}