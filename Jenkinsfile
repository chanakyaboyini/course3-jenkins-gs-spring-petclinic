pipeline {
    agent any

    // Automatically installs/configures Maven from Global Tool Configuration
    tools {
        maven 'maven3'
    }

    // Trigger this pipeline on every GitHub push
    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone whatever repo/branch triggered this build
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Package Artifacts') {
            steps {
                // Build your JAR/WAR without running tests
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Resolve your Maven tool installation
                    def mvnHome = tool 'maven3'
                    // withSonarQubeEnv injects SONAR_HOST_URL & SONAR_AUTH_TOKEN
                    withSonarQubeEnv('LocalSonarQube') {
                        sh """
                            ${mvnHome}/bin/mvn clean verify sonar:sonar \
                              -Dsonar.projectKey=Spring-PetClinic \
                              -Dsonar.login=${env.SONAR_AUTH_TOKEN} \
                              -Dsonar.host.url=${env.SONAR_HOST_URL}
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            echo '🛠️ Pipeline finished—clean up or notify as needed.'
        }
    }
}