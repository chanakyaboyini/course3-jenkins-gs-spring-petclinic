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
        stage('build') {
            steps {
                // Run Maven on a Unix agent.
                sh "./mvnw.cmd clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage('Integraion test') {
            steps {
                script {
                    mvnIntegrationTest()
                }
            }
        }
    }
}
