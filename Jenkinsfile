@library('my-shared-library') _

pipeline {
    agent any

    stages {
        stage('checkout') {
            steps {

                script{
                // Get some code from a GitHub repository
                gitCheckout(
                    branch: "main",
                    url: "https://github.com/chanakyaboyini/shared_library.git"
                )
            }
        }
        }
    }
}