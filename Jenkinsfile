@Library('my-shared-library') _

pipeline {
    agent any

    stages {
        stage('checkout') {
            steps {
                script {
                    // Get some code from a GitHub repository
                    gitCheckout(
                        branch: 'main',
                        url: 'https://github.com/chanakyaboyini/course3-jenkins-gs-spring-petclinic.git'
                    )
                }
            }
        }
        stage('Unit Test Maven') {
            steps {
                script {
                    
                    mvnTest()
                }
            }
        }
    }
}
