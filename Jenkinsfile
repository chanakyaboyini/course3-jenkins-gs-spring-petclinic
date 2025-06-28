pipeline {
    agent any

    tools {
        maven 'maven3' // Name should match your Maven installation in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Package Artifacts') {
            steps {
                sh 'mvn package'
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_HOST_URL = 'http://192.168.29.163:9000' // Replace with your SonarQube host URL
                SONAR_AUTH_TOKEN = credentials('LocalSonarQube') // Credential ID in Jenkins
            }
            steps {
                sh '''mvn sonar:sonar Dsonar.projectKey=your_project_key Dsonar.host.url="$SONAR_HOST_URL" Dsonar.login="$SONAR_AUTH_TOKEN"'''
            }
        }
    }
}