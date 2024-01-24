pipeline{
    agent any

    environment{
        PATH= "/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin:$PATH"
    }

    stages{
        stage("git_clone"){
            steps{
                git branch: 'main', credentialsId: 'c0bc5748-93ef-489c-9e1d-bc087359823d', url: 'https://github.com/aakumar07/DevOps.git'
            }
        }
        stage("mvn and sonar"){
            steps{
                sh "mvn -f /var/lib/jenkins/workspace/pipline/Maven/webpage_feture/pom.xml clean sonar:sonar package"
            }
        }
        stage("Nexus Artifact Uploader"){
            steps{
                nexusArtifactUploader artifacts: [
		 [
		   artifactId: 'webpage_feture', 
		   classifier: '', 
		   file: '/var/lib/jenkins/workspace/pipline/Maven/webpage_feture/target/webpage_feture.war', 
		   type: 'war'
		 ]
		], 
		credentialsId: 'nexus3', 
		groupId: 'sample_project', 
		nexusUrl: '54.64.49.78:8081', 
		nexusVersion: 'nexus3', 
		protocol: 'http', 
		repository: 'sample-snapshot', 
		version: 'snap'
            }
        }
        stage("Tomcat deploy"){
            steps{
                sshagent(['tomcat']) {
                    sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipline/Maven/webpage_feture/target/webpage_feture.war ec2user@3.144.139.11:/home/ec2-user/tomcat-9/webapps/"
                }
            }
        }
    }
}
