pipeline {
  agent any

  tools {
    maven 'maven3'
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
        nexusArtifactUploader(
          nexusVersion  : 'nexus3',
          protocol      : 'http',
          nexusUrl      : '3.93.9.212:8081',
          credentialsId : 'your-jenkins-credentials-id',
          groupId       : 'org.springframework.samples',
          version       : '3.1.0',
          repository    : 'Spring-Clinic',
          artifacts     : [
            [
              artifactId : 'Spring-Clinic',
              classifier : '',
              file       : 'target/Spring-Clinic-3.1.0.jar',
              type       : 'jar'
            ]
          ]
        )
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