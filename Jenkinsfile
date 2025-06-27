pipeline {
  agent any 

  tools {
    maven 'maven3'               // your Maven install
    sonarScanner 'SonarScannerCLI' // the CLI you just added
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Compile') {
      steps { sh 'mvn clean compile' }
    }
    stage('Package') {
      steps { sh 'mvn package' }
    }
    stage('SonarQube Analysis') {
      steps {
        // this MATCHES the name in Configure System
        withSonarQubeEnv('LocalSonarQube') {
          // 'tool' returns the install dir of your SonarScannerCLI
          script {
            def scannerHome = tool 'SonarScannerCLI'
            sh """
              ${scannerHome}/bin/sonar-scanner \
                -Dsonar.projectKey=myproject \
                -Dsonar.sources=. \
                -Dsonar.host.url=$SONAR_HOST_URL \
                -Dsonar.login=$SONAR_AUTH_TOKEN
            """
          }
        }
      }
    }
  }
}