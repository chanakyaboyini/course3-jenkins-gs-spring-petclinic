pipeline {
  agent any

  tools {
    maven 'maven3'                 // your Maven install name
    sonarRunner 'SonarScannerCLI'  // <-- use sonarRunner, not sonarScanner
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Compile & Package') {
      steps {
        sh 'mvn clean package -DskipTests' 
      }
    }

    stage('SonarQube Analysis') {
      steps {
        // must match the "Name" in Configure System → SonarQube servers
        withSonarQubeEnv('LocalSonarQube') {
          // the tools block injects SONAR_RUNNER_HOME for you
          sh """
            $SONAR_RUNNER_HOME/bin/sonar-scanner \
              -Dsonar.projectKey=myproject \
              -Dsonar.sources=. \
              -Dsonar.host.url=$SONAR_HOST_URL \
              -Dsonar.login=$SONAR_AUTH_TOKEN
          """
        }
      }
    }

    stage('Quality Gate') {
      steps {
        timeout(time: 2, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
      }
    }
  }
}