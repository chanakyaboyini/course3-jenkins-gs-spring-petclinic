pipeline {
    agent any

    tools {
        maven 'maven3'    // must match your Jenkins Maven tool name
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

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Test Results') {
            steps {
                junit '**/target/surefire-reports/*.xml'
            }
        }

        stage('Cleanup') {
            steps {
                cleanWs()   // clean workspace after build
            }
        }
    }

    post {
        always {
            // archive the built jars and test results every run
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            junit '**/target/surefire-reports/*.xml'
        }
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}