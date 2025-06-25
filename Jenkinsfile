pipeline {
    agent any

    // Automatically installs/configures Maven from Global Tool Configuration
    tools {
        maven 'maven3'
    }

    // Trigger this pipeline on every GitHub push
    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone whatever repo/branch triggered this build
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Package Artifacts') {
            steps {
                // Build your JAR/WAR without running tests
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
    def mvn = tool 'Default Maven';
    withSonarQubeEnv() {
      sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=Spring-PetClinic -Dsonar.projectName='Spring-PetClinic'"
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