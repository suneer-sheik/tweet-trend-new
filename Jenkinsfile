def registry = 'https://suneer.jfrog.io'
pipeline {
    agent any

    environment {
        PATH = "/opt/apache-maven-3.9.9/bin:$PATH"
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage ("test") {
            steps{
                echo "------------- unit test started -------------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "------------- unit test completed ----------"
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
             
        stage("Jar Publish") {
            steps {
                script {
                    echo '<--------------- Jar Publish Started --------------->'
                    try {
                        // Create Artifactory server connection
                        def server = Artifactory.newServer url:registry + "/artifactory", credentialsId: "artifact_cred"
                        
                        // Define properties to attach to the artifact
                        def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}"
                        
                        // Print debug log of the uploadSpec
                        echo "Upload spec: ${properties}"
                        
                        def uploadSpec = """{
                            "files": [
                                {
                                    "pattern": "jarstaging/(*)", // Adjust this if your file pattern doesn't match
                                    "target": "libs-release-local/{1}",
                                    "flat": "false",
                                    "props" : "${properties}",
                                    "exclusions": [ "*.sha1", "*.md5" ]
                                }
                            ]
                        }"""
                        
                        // Log the uploadSpec to verify the structure
                        echo "Uploading with spec: ${uploadSpec}"

                        // Upload artifact to Artifactory
                        def buildInfo = server.upload(uploadSpec)
                        buildInfo.env.collect()

                        // Publish build info
                        server.publishBuildInfo(buildInfo)
                        echo '<--------------- Jar Publish Ended --------------->'
                    } catch (Exception e) {
                        echo "Error during Artifactory upload: ${e.message}"
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }  
    }
}