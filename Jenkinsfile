pipeline {
   agent any
	environment {
	WEBEX_TOKEN = credentials('webex-bot-token')
	WEBEX_ROOM_ID = 'Y2lzY29zcGFyazovL3VybjpURUFNOnVzLXdlc3QtMl9yL1JPT00vNTVmNWIwODAtYzY3YS0xMWYwLWIxZmItZDdkYjUzMDk3ZWQ4'
}
    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/mejilett/jenkins'
            }
        }
        stage('Build') {
            steps {
                sh 'chmod +x build.sh' // Make build.sh executable
                sh './build.sh'
            }
        }
        stage('Test') {
            steps {
                sh 'chmod +x test.sh' // Make test.sh executable
                sh './test.sh'
            }
        }

    }
	post {
		always {
			script {
			def status = currentBuild.currentResult
			def message = "Build #${BUILD_NUMBER} finished. Status: ${status}"
			node {
			sh """
				curl -X POST 'https://webexapis.com/v1/messages' \
				-H 'Authorization: Bearer ${env.WEBEX_TOKEN}' \
				-H 'Content-Type: application/json' \
				-d '{"roomId":"${env.WEBEX_ROOM_ID}", "text": "${message}"}'
			"""
			}
			}
		}
	}
}