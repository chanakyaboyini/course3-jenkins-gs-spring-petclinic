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
            steps {
                withSonarQubeEnv('LocalSonar') {
                    sh '''sonar-scanner -Dsonar.projectKey=myproject -Dsonar.sources=. Dsonar.host.url=http://localhost:9000 Dsonar.login=your_token'''
                }
            }
        }
    }
}