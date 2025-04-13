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
        stage("Clean & Build"){
            steps{
                sh '''
                cd ${WORKSPACE}
                rm -rf ~/m2/repository/org/jacoco/org.jacoco.agent
                /opt/apache-maven-3.9.9/bin/mvn clean install -U
            '''
            }
        }            
    }
}