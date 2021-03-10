pipeline {
	environment {
		registry = "banshee77/simple-node-js-react-npm-app"
			registryCredential = 'dockerhub_id'
			dockerImage = ''
	}
	agent any
	stages {
		stage('Cloning our Git') {
			steps {
				git 'https://github.com/banshee77/simple-node-js-react-npm-app.git'
			}
		}
		stage('Building our image') {
			steps {
				script {
					dockerImage = docker.build registry + ":$BUILD_NUMBER"
				}
			}
		}
		stage('Deploy our image') {
			steps {
				script {
					docker.withRegistry('', registryCredential) {
						dockerImage.push()
					}
				}
			}
		}
		stage('Cleaning up') {
			steps {
				bat "docker rmi $registry:$BUILD_NUMBER"
			}
		}
	}
}