pipeline {
    agent {
        node{
            label 'maven'
        }
    }
environment{
    PATH = "/opt/apache-maven-3.9.9/bin:$PATH"
}

    stages {
        stage("Clean Workspace"){
            steps{
                cleanWs()
            }
        }            
    
        stage('Build'){
            steps{
                sh '/opt/apache-maven-3.9.9/bin/mvn clean install -U'
            }
        }            
    }
}