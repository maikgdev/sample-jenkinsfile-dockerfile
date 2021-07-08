#!groovy

project_name = 'sample'
uniqueId = "${env.BUILD_NUMBER}"

node() {
    def appFrontend
    try {	
		stage('Cleanup') {
			cleanWs()
		}
	
        stage('Checkout') {
            
        }

        stage('D:Image') {
			appFrontend = docker.build("${project_name}:${uniqueId}-fe", "-f ./${project_name}/Dockerfile .")
        }
		
		stage ('D:B&D Frontend') {
			appFrontend.inside ("-u 0:0") {
				dir(project_name)  {
					sh '''
                       node index.js
                   	'''
				}
			}
		}
		
		stage ('D:Tests') {			
			//TODO Implement Tests
		}

		//office365ConnectorSend message: "**Build succeeded**", webhookUrl: "${teams_notification_url}", color: '4BB543'
		currentBuild.result = 'SUCCESS'

    } catch (e) {
		currentBuild.result = 'FAILURE'		
        throw e
    } finally {
		notifyBitbucket()
	}
}
