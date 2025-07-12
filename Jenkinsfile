pipeline {
  agent any

  tools {
    maven 'maven3'
  }

  environment {
    // Bind credentials so we can use them in shell or settings.xml
    NEXUS_CRED = credentials('nexus-deployer')
  }

  stages {
    stage('Validate Nexus Access') {
      steps {
        echo "Using Nexus user: ${NEXUS_CRED_USR}"
        sh "curl -u ${NEXUS_CRED_USR}:${NEXUS_CRED_PSW} http://${NEXUS_HOST}:8081/service/rest/v1/status"
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

          // Choose one of two methods:

          // Method A: Nexus Artifact Uploader Plugin
          nexusArtifactUploader(
            nexusVersion       : 'nexus3',
            protocol           : 'http',
            nexusUrl           : "${env.NEXUS_HOST}:8081",
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

          // Method B: Native Maven Deploy
          /*
          sh """
            mvn deploy \
              -DskipTests \
              -s ${configFile('nexus-settings')} \
              -DaltDeploymentRepository=spring-clinic::default::http://${NEXUS_HOST}:8081/repository/Spring-Clinic
          """
          */
        }
      }
    }
  }

  post {
    success { echo '✅ Pipeline completed and artifact deployed to Nexus.' }
    failure { echo '❌ Pipeline failed – inspect console logs for details.' }
  }
}