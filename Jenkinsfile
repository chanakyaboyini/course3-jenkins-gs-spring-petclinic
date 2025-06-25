pipeline {
    agent any

    // The tools block sets up Maven so that environment variables are automatically configured.
    tools {
        maven 'maven3'  // Make sure this name matches your Maven installation in Global Tool Configuration.
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
                // Package the artifacts (e.g., JAR, WAR, etc.).
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Look up the SonarQube Scanner installation.
                    // Make sure the tool name 'Sonar-scanner' matches your Global Tool Configuration,
                    // and that its type is set to 'hudson.plugins.sonar.SonarRunnerInstallation'.
                    def scannerHome = tool name: 'LocalSonarQube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    
                    // withSonarQubeEnv injects necessary env variables based on the SonarQube server configuration.
                    // Replace "LocalSonarQube" with the name you provided in Jenkins' Configure System.
                    withSonarQubeEnv('LocalSonarQube') {
                        // Run SonarQube Scanner using the path from the tool step.
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=YourProjectKey -Dsonar.sources=src"
                    }
                }
            }
        
            }
        }
    }