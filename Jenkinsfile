pipeline {
  agent any

  tools {
    maven 'maven3'               // your Jenkins Maven tool name
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

    stage('Build & Parse POM') {
      steps {
        sh 'mvn clean package -DskipTests'

        script {
          // parse pom.xml
          def pom = readMavenPom file: 'pom.xml'
          env.GROUP_ID     = pom.groupId
          env.ARTIFACT_ID  = pom.artifactId
          env.VERSION      = pom.version
          // choose repo
          env.REPO = env.VERSION.endsWith('-SNAPSHOT') 
                      ? 'maven-snapshots' 
                      : 'maven-releases'
          // artifact filename
          env.FILE = "target/${env.ARTIFACT_ID}-${env.VERSION}.jar"
        }
      }
    }

    stage('Publish to Nexus') {
      steps {
        nexusArtifactUploader(
          nexusVersion:   'nexus3',
          protocol:       'http',
          nexusUrl:       env.NEXUS_URL,
          credentialsId:  env.NEXUS_CRED_ID,
          repository:     env.REPO,
          groupId:        env.GROUP_ID,
          artifactId:     env.ARTIFACT_ID,
          version:        env.VERSION,
          type:           'jar',
          file:           env.FILE
        )
      }
    }
  }

  post {
    success {
      echo "✅ Successfully published ${env.ARTIFACT_ID}-${env.VERSION}.jar to ${env.REPO}"
    }
    failure {
      echo "❌ Pipeline failed – check the console logs above"
    }
  }
}