pipeline {
    agent any

    tools {
        // Ensure the Maven installation name matches what’s configured in Global Tool Configuration.
        maven 'maven3'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Check out code from your chosen SCM.
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Inject SonarQube environment variables based on the server configuration named 'LocalSonarQube'
                    withSonarQubeEnv('LocalSonarQube') {
                        // First build the project: clean and package so that .class files are generated.
                        sh "mvn clean package"
                        
                        // Next, run the SonarQube analysis.
                        // The property sonar.java.binaries tells SonarQube where your compiled classes are.
                        sh "mvn sonar:sonar -Dsonar.projectKey=YourProjectKey -Dsonar.sources=src -Dsonar.java.binaries=target/classes"
                    }
                }
            }
        }
        stage('Quality Gate') {
            steps {
                // Wait up to 1 minute for the SonarQube Quality Gate result.
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}