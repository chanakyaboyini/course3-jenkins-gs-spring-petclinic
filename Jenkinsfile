pipeline {
  agent any
  tools { maven 'maven3' }

  stages {
    stage('Validate Credentials') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'nexus-deployer',   // ← your real Jenkins Cred ID
          usernameVariable: 'NEXUS_USR',
          passwordVariable: 'NEXUS_PSW'
        )]) {
          echo "Using Nexus user: ${NEXUS_USR}"
        }
      }
    }

    stage('Build & Package') {
      steps {
        sh 'mvn clean package -DskipTests'
        stash includes: 'target/*.jar', name: 'app-jar'
      }
    }

    stage('Deploy to Nexus') {
      steps {
        unstash 'app-jar'
        script {
          def jarPath = findFiles(glob: 'target/*.jar')[0].path

          nexusArtifactUploader(
            nexusVersion  : 'nexus3',
            protocol      : 'http',
            nexusUrl      : '3.93.9.212:8081',
            credentialsId : 'Spring-Clinic',    // ← ensure this matches the Validate step
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
    success { echo '✅ Deployed to Nexus!' }
    failure { echo '❌ Deployment failed – check logs.' }
  }
}