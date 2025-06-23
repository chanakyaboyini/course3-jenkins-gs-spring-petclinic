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
        stage('Build') {
            steps {
                echo 'Executing build steps...'
                // Add your build logic here.
            }
        }
        stage('Compile') {
            steps {
                // Compile the code.
                sh 'mvn clean compile'
            }
        }
    }
}