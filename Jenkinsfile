CODE_CHANGES = getGitChanges()
pipeline { // must be top-level
	
	agent any // where to execute
	tools {
		maven 'Maven'
	}
	parameters {
		string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod')
		choice(name: 'VERSION', choices: ['1.1.0','1.2.0','1.3.0'], description: '')
		booleanParam(name: '', defaultValue: true, description: '')
	}
	environment {
		NEW_VERSION = '1.3.0'
		SERVER_CREDENTIALS = credentials('server-credentials')
	}

	stages {
		stage("init") { //

			steps {
			  script {
					gv = load "script.groovy"
				}
			}
		}	
		stage("build") {

			steps {
			  //command
				echo 'building the application...'
				echo "building version ${NEW_VERSION}"
			}
		}

		stage("test") {
			when {
				expression {
					BRANCH_NAME == 'dev' || BRANCH_NAME == 'master'
				}
			}

			steps {
				echo 'test the application...'
				script {
					gv.testApp()
				}			
			}
		}

		stage("deploy") { //
			steps {
				echo 'deploy the application...'

				script {
					gv.testApp()
				}		
				//param
				echo "deploying with ${params.VERSION}"

				//env
				echo "deploying with ${SERVER_CREDENTIALS}"
				sh "${SERVER_CREDENTIALS}"		

				//crediantils binding
				withCredentials([
					usernamePassword(credentials: 'server-credentials', usernameVariable: USER, passwordVariable: PWD)
				]) {
					sh "some script ${USER} ${PWD}"
				}
			}
		}
	}
	post {
		always {
			//
		}
		failure {
			//
		}
	}

}

