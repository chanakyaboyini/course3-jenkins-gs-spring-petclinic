pipeline {
  agent any

  tools {
    maven 'maven3'
  }

  triggers {
    githubPush()
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        script {
          // Resolve the Maven home we declared above
          def mvnHome = tool 'maven3'

          // This block injects SONAR_HOST_URL and SONAR_AUTH_TOKEN
          // based on the SonarQube server named "LocalSonarQube"
          withSonarQubeEnv('LocalSonarQube') {
            sh """
              ${mvnHome}/bin/mvn sonar:sonar \
                -Dsonar.projectKey=Spring-PetClinic \
                -Dsonar.login=${env.SONAR_AUTH_TOKEN} \
                -Dsonar.host.url=${env.SONAR_HOST_URL}
            """
          }
        }
      }
    }
  }

  post {
    always {
      echo '🛠️ Pipeline finished—clean up or notify as needed.'
    }
  }
}