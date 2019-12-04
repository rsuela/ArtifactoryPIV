pipeline {
    agent any
    stages {
        stage('Clone'){
            steps {
                git url: 'https://github.com/rsuela/ArtifactoryPIV.git'
            }
        }

        stage('Artifactory download and upload'){
            steps {
                script{
                    // Obtain an Artifactory server instance, defined in Jenkins --> Manage:
                    def server = Artifactory.server 'artifactory'

                    // Read the download and upload specs:
                    def downloadSpec = readFile 'artifactory/resources/props-download.json'
                    def uploadSpec = readFile 'artifactory/resources/props-upload.json'

                    // Download files from Artifactory:
                    def buildInfo1 = server.download spec: downloadSpec
                    // Upload files to Artifactory:
                    def buildInfo2 = server.upload spec: uploadSpec
                  
                    def exists = fileExists 'Bazinga/ArtifactoryPipeline.zip'

                    if (exists) {
    	                sh "echo File exist"
                    } else {
	                    echo "File does not exist"
                    	sh 'exit 1'
                    }
                  
                    // Merge the local download and upload build-info instances:
                    buildInfo1.append buildInfo2

                    // Publish the merged build-info to Artifactory
                    server.publishBuildInfo buildInfo1
                }
            }
        }
    }
}
