pipeline {
    agent any
	environment {
        PATH = "$PATH:/opt/apache-maven-3.6.3/bin"
    }
    stages{
        stage('GetCode'){
            steps{
                git 'https://github.com/RatiRanjanDas/simple-app.git'
            }
         }
		stage('Build'){
            steps{
                 sh script: 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "simpleapp-snapshot" : "simpleapp-release"
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'simple-app', 
                            classifier: '', 
                            file: "target/simple-app-${mavenPom.version}.war", 
                            type: 'war'
                        ]
                    ], 
                    credentialsId: 'nexus3', 
                    groupId: 'in.javahome', 
                    nexusUrl: '52.66.200.2:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'jenkins-repo', 
                    version: "${mavenPom.version}"
                    }
            }
        }
    }
}
