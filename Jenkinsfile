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
          nexusVersion: 'nexus3',           // OSS v3
          protocol: 'http',
          nexusUrl: 'localhost:8082',       // your Nexus host:port
          credentialsId: 'Spring-Clinic',   // ID of your Jenkins-stored Nexus creds
          repository: 'maven-releases',     // the hosted repo name you created
          groupId: 'com.example',
          version: '3.1.0',
          artifacts: [
            [
              artifactId: 'my-app',
              type: 'jar',
              file: 'target/spring-petclinic-3.1.0-SNAPSHOT.jar.original'
            ]
          ]
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