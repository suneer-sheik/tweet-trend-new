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
        stage('Build') {
            steps {
                echo "---------- build started ------------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "---------- build completed -----------"
            }
        }

        stage('test'){
            steps{
                echo "---------- unit test started ------------"
                sh 'mvn surefire-report:report'
                echo "---------- unit test completed -----------" 
            }
        }

        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'suneer-sonar-scanner'
            }
            steps {
                withSonarQubeEnv('suneer-sonarqube-server') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}
