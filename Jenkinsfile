pipeline {
    agent any

    tools {
        maven 'Maven3' // Make sure the name matches your Global Tool Configuration
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
    }
}