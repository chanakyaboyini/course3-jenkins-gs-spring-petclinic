pipeline {
    agent any

    tools {
        maven 'maven3' // Make sure the name matches your Global Tool Configuration
        sonarQubeScanner 'Sonar-scanner'

    }

    stages {
        stage('Checkout') {
            steps {
                // Automatically checks out the repository tied to this build.
                checkout scm
            }
        }
        stage('Compile') {
            steps {
                // Compile the entire project.
                sh 'mvn clean compile'
            }
        }
        stage('Package Artifacts') {
            steps {
                // Package the artifacts (this might create a JAR, WAR, etc.).
                sh 'mvn package'
            }
        }
        stage('Static Code Analysis') {
            steps {
                // Run SonarQube analysis using Maven.
                withSonarQubeEnv('Sonar-scanner') {
                    // Using the Maven Sonar plugin defined in your pom.xml.
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                // Wait for SonarQube to provide a Quality Gate result.
                // This step waits up to 1 minute (adjust as needed) and aborts the build if quality gate fails.
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}