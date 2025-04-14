pipeline {
    agent any

    environment {
        PATH = "/opt/apache-maven-3.9.9/bin:$PATH"
    }

    tools {
        maven 'maven-3.9.9'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'suneer-sonar-scanner'
                    withSonarQubeEnv('suneer-sonarqube-server') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }
}
