pipeline {
  agent any

  tools { maven 'maven3' }

  environment {
    // Inject your Sonar token from Jenkins Credentials
    SONAR_TOKEN = credentials('sonar-token')
  }

  stages {
    stage('Checkout') { steps { checkout scm } }
    stage('Build')    { steps { sh 'mvn clean package -DskipTests' } }

    stage('SonarQube Analysis') {
      environment { SONAR_SCANNER_OPTS = '-Xmx512m' }
      steps {
        withSonarQubeEnv('LocalSonarQube') {
          sh """
            ${tool 'LocalSonarQube'}/bin/sonar-scanner \
              -Dsonar.host.url=${env.SONAR_HOST_URL} \
              -Dsonar.login=${env.SONAR_TOKEN} \
              -Dsonar.projectKey=YourProjectKey \
              -Dsonar.sources=src \
              -Dsonar.java.binaries=target/classes
          """
        }
      }
    }
  }

  post {
    always { echo 'Pipeline complete.' }
  }
}