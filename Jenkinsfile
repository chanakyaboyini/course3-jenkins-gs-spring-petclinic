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

  }
