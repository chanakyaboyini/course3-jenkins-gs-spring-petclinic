pipeline {
    agent any

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
    }
}
