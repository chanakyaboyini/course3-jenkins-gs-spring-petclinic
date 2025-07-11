pipeline {
  agent any

  tools {
    maven 'maven3'
  }

  stages {
    stage('Build & Package') {
      steps {
        sh 'mvn clean package -DskipTests'
        stash includes: 'target/*.jar', name: 'app-jar'
      }
    }

    stage('Deploy to Nexus') {
      steps {
        unstash 'app-jar'

        // dynamically locate the JAR, in case the name/casing changes
        script {
          def jarPath = findFiles(glob: 'target/*.jar')[0].path

          nexusArtifactUploader(
            nexusVersion  : 'nexus3',
            protocol      : 'http',
            nexusUrl      : '3.93.9.212:8081',
            credentialsId : 'nexus-deployer',       // ← replace with your real Jenkins credentials ID
            groupId       : 'org.springframework.samples',
            version       : '3.1.0',
            repository    : 'Spring-Clinic',
            artifacts     : [[
              artifactId : 'spring-clinic',
              classifier : '',
              file       : jarPath,
              type       : 'jar'
            ]]
          )
        }
      }
    }
  }

  post {
    success {
      echo '✅ Pipeline completed successfully.'
    }
    failure {
      echo '❌ Pipeline failed – check the console logs.'
    }
  }
}