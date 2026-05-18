pipeline {
    agent any
	// this is jenkin comments

	/*
	this is multi line comment
	line1
	line 2
	*/
    stages {
        stage('Build') {
			agent {
				docker {
					image 'node:18-alpine'
					reuseNode true
				}
			}
            steps {
                sh '''
					ls -la
					npm --version
					node --version
					npm ci
					npm run build
					ls -la
				'''
            }
        }
		stage('Test') {
			agent {
				docker {
					image 'node:18-alpine'
					reuseNode true
				}
			}
			steps {
				sh '''
					# test -f build/index.html #this is shell comment
					npm test
				'''
			}
		}
	}
	post {
		always {
			junit 'test-results/junit.xml'
		}
	}
}
