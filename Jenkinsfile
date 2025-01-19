

pipeline {
    agent any

    stages {
        stage('checkout') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/chanakyaboyini/course3-jenkins-gs-spring-petclinic.git'
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
        stage('archive') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar'
            }
        
        
        }
        stage('test') {
            steps {
                junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
            }
        }
        stage('code coverage') {
            steps {
                jacoco()
            }
        }
    }
}
