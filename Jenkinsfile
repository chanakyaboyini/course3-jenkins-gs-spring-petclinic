pipeline {
  agent any

  tools {
    maven 'maven3'
    sonarScanner 'LocalSonarQube'    // <-- name from Global Tool Configuration
  }

  stages {
    // … checkout, compile, package …

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('LocalSonarQube') {
          // 'SONAR_SCANNER_HOME' is injected for you by the tools block
          sh """
            $SONAR_SCANNER_HOME/bin/sonar-scanner \
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