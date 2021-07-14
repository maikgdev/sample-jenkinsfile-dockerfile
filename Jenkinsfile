#!groovy

project_name = 'sample'
uniqueId = "${env.BUILD_NUMBER}"

node() {
    def appFrontend
    try {	
	stage('Cleanup') {
		//cleanWs()
	}
	
        stage('Checkout') {
            sh 'docker version'
            checkout scm
        }

        stage('D:Image') {
            appFrontend = docker.build("${project_name}:${uniqueId}", "-f Dockerfile .")
        }
		
	stage ('D:B&D Frontend') {
		appFrontend.inside ("-u 0:0") {
			sh '''
       				node index.js
			'''
		}
	}

	stage ('D:Tests') {			
		//TODO Implement Tests
	}
    
	stage('Publish')  {
	    def Creds = "d69b67e6-961c-40fa-b739-aae404c023f3"
	    withDockerRegistry([credentialsId: "${Creds}", url: 'https://index.docker.io/v1/']) {
		sh "docker tag ${project_name}:${uniqueId} blizzard2k8/jenkins-docker-publish:${uniqueId} "
            	sh "docker push blizzard2k8/jenkins-docker-publish:${uniqueId}"
	    }
	}


	//office365ConnectorSend message: "**Build succeeded**", webhookUrl: "${teams_notification_url}", color: '4BB543'
	currentBuild.result = 'SUCCESS'

    } catch (e) {
		currentBuild.result = 'FAILURE'		
        throw e
    } finally {
		//notifyBitbucket()
    }
}
