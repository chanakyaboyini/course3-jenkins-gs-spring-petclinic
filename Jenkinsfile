pipeline {
  agent any

  tools {
    maven 'maven3'               // must match your Jenkins Maven installation name
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Compile') {
      steps {
        sh 'mvn clean compile'
      }
    }

    stage('Package Artifacts') {
      steps {
        sh 'mvn package -DskipTests'
      }
    }

    stage('Publish to Nexus') {
      steps {
        nexusArtifactUploader(
          nexusVersion: 'nexus3',
          protocol: 'http',
          nexusUrl:   'http://localhost:8082',  // adjust if using a Docker network alias
          credentialsId: 'nexus-admin',
          repository:    'maven-releases',
          groupId:       'com.example',
          artifactId:    'spring-petclinic',
          version:       '3.1.0-SNAPSHOT',
          type:          'jar',
          file:          'target/spring-petclinic-3.1.0-SNAPSHOT.jar'
        )
      }
    }
  }

  post {
    success {
      echo 'Pipeline completed successfully!'
    }
    failure {
      echo 'Pipeline failed. Please check the logs.'
    }
  }
}