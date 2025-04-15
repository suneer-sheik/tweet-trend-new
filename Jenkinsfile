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
                withSonarQubeEnv('suneer-sonarqube-server') {  // If you have configured more than one global server connection, you can specify its name
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}