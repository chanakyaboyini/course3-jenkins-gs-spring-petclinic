pipeline {
  agent any

  tools { maven 'maven3' }

  stages {
    /* […] your other stages here […] */

    stage('Docker Build') {
      steps {
        script {
          docker.image('maven:3.6.3-jdk-8').inside {
            sh 'mvn clean'
          }
        }
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      junit '**/target/surefire-reports/*.xml'
    }
    success { echo 'Build completed successfully!' }
    failure { echo 'Build failed!' }
  }
}