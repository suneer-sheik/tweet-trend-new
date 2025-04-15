pipeline {
    agent {
        node {
            label 'maven'
        }
    }

environment {
    PATH = "/opt/apache-maven-3.9.9/bin:$PATH"
}

    stages {
        stage("Build") {
            steps {
                echo "----------- Build Started ----------"
                sh 'mvn clean deploy'
                echo "----------- Build Completed ----------"
            }
        }

        stage("SonarQube Analysis") {
            environment {
                scannerHome = tool name: 'suneer-sonar-scanner'
            }
            steps {
                echo "----------- SonarQube Analysis Started ----------"
                withSonarQubeEnv('suneer-sonarqube-server') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
                echo "----------- SonarQube Analysis Completed ----------"
            }
        }
    }
}