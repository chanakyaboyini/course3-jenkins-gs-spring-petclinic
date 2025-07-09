pipeline {
  agent any

  tools {
    maven 'maven3'               // your Jenkins Maven tool name
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
          nexusVersion:   'nexus3',
          protocol:       'http',
          nexusUrl:       'localhost:8082',  // or http://nexus:8081 if on Docker network
          credentialsId:  'Spring-Clinic',
          repository:     'maven-releases',
          groupId:        'com.example',
          version:        '3.1.0-SNAPSHOT',
          artifacts: [
            [
              artifactId: 'spring-petclinic',
              type:       'jar',
              file:       'target/spring-petclinic-3.1.0-SNAPSHOT.jar'
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