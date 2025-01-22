@Library('my-shared-library') _

pipeline {
    agent any

    parameters {
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'test', 'prod'], description: 'Target environment')
    }

    stages {
        stage('checkout') {
            steps {
                script {
                    // Get some code from a GitHub repository
                    gitCheckout(
                        branch: "${params.BRANCH}",
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
        stage('Integration test') {
            steps {
                script {
                    mvnIntegrationTest()
                }
            }
        }
        stage('Static Code Analysis') {
            steps {
                script {
                    def SonarQubecredentialsId = 'Jenkins_Sonar_test_practice'
                    staticCodeAnalysis(SonarQubecredentialsId)
                }
            }
        }
        stage('Maven Build : Build') {
            steps {
                script {
                    mvnBuild()
                }
            }
        }
    }
}
