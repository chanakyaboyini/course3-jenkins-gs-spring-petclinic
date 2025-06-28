pipeline {
  agent any

  tools {
    maven 'maven3'    // must match your Jenkins Maven tool name
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
        sh 'mvn package'
      }
    }

    stage('SonarQube Analysis') {
      environment {
        SONAR_HOST_URL   = 'https://192.168.29.163:9000'
        SONAR_AUTH_TOKEN = credentials('LocalSonarQube')
      }
      steps {
        sh '''
          mvn sonar:sonar \
            -Dsonar.projectKey=your_project_key \
            -Dsonar.host.url="${SONAR_HOST_URL}" \
            -Dsonar.login="${SONAR_AUTH_TOKEN}"
        '''
      }
    }
  }
}