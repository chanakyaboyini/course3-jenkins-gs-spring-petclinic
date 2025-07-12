pipeline {
  agent any

  tools {
    maven 'maven3'
  }

  environment {
    // Jenkins credentials ID for your Nexus deploy user
    NEXUS_CRED = credentials('nexus-deployer')
    // Host (IP or DNS) and port of your AWS Nexus instance
    NEXUS_HOST = '13.218.71.63:8081'
  }

  stages {
    stage('Validate Nexus Access') {
      steps {
        echo "Using Nexus user: ${NEXUS_CRED_USR}"
        // now using HTTPS
        sh "curl -u ${NEXUS_CRED_USR}:${NEXUS_CRED_PSW} https://${NEXUS_HOST}/service/rest/v1/status"
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
          // locate the built JAR
          def jarPath = findFiles(glob: 'target/*.jar')[0].path

          nexusArtifactUploader(
            nexusVersion       : 'nexus3',
            protocol           : 'https',                 // switch to HTTPS
            nexusUrl           : NEXUS_HOST,
            credentialsId      : 'nexus-deployer',
            groupId            : 'org.springframework.samples',
            version            : '3.1.0',
            repository         : 'Spring-Clinic',
            snapshotRepository : 'Spring-Clinic-snapshots',
            artifacts          : [[
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
      echo '✅ Pipeline completed and artifact deployed to Nexus.'
    }
    failure {
      echo '❌ Pipeline failed – inspect console logs for details.'
    }
  }
}