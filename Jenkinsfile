pipeline {
  agent any

  tools {
    maven 'maven3'            // your Jenkins Maven tool name
  }

  environment {
    NEXUS_URL     = 'http://localhost:8082'  // or http://nexus:8081 on Docker network
    NEXUS_CRED_ID = 'nexus-admin'            // must match your Jenkins credential ID
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

    stage('Publish to Nexus') {
      steps {
        script {
          // Parse the POM for GAV coords
          def pom       = readMavenPom file: 'pom.xml'
          def groupId   = pom.groupId
          def artifact  = pom.artifactId
          def version   = pom.version
          // Choose snapshot vs release repo
          def repo      = version.endsWith('-SNAPSHOT') 
                            ? 'maven-snapshots' 
                            : 'maven-releases'
          def jarFile   = "target/${artifact}-${version}.jar"

          // Upload to Nexus
          nexusArtifactUploader(
            nexusVersion:  'nexus3',
            protocol:      'http',
            nexusUrl:      env.NEXUS_URL,
            credentialsId: env.NEXUS_CRED_ID,
            repository:    repo,
            groupId:       groupId,
            artifactId:    artifact,
            version:       version,
            type:          'jar',
            file:          jarFile
          )
        }
      }
    }
  }

  post {
    success {
      echo "✅ Published ${env.ARTIFACT_ID}-${env.VERSION}.jar to ${env.REPO}"
    }
    failure {
      echo "❌ Pipeline failed – check the console logs."
    }
  }
}