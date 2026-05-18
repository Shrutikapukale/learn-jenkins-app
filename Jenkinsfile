pipeline {
    agent any
	environment {
		NETLIFY_SITE_ID = '171024ea-02a8-4052-aec8-7faed2f46b69'
		NETLIFY_AUTH_TOKEN = credentials('netlify-token')
	}
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
		
		stage ('Tests'){
			parallel{
				stage('Unit Tests') {
					agent {
						docker {
							image 'node:18-alpine'
							reuseNode true
						}
					}
					steps {
						sh '''
							test -f build/index.html
							npm test
						'''
					}
					
					post {
						always {
							junit 'jest-results/junit.xml'
						}
					}

				}
				
				stage('E2E Test') {
					agent {
						docker {
							image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
							reuseNode true
						}
					}
					steps {
						sh '''
							npm install serve
							node_modules/.bin/serve -s build &
							sleep 10
							npx playwright test --reporter=html
						'''
					}
					
					post {
						always {
							publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright local Report', reportTitles: '', useWrapperFileDirectly: true])
							}
						}
				}
					
					
			}
		}

		stage('Deploy stage') {
			agent {
				docker {
					image 'node:18-alpine'
					reuseNode true
				}
			}
            steps {
                sh '''
				npm install netlify-cli@20.1.1
                node_modules/.bin/netlify --version
				echo "Deploying in staging SITE ID: $NETLIFY_SITE_ID"
				node_modules/.bin/netlify status
				node_modules/.bin/netlify deploy --dir=build
				'''
            }
			
		}
		stage ('Approval') {
			steps {
				timeout (time: 15, UNIT: MINUTES){
					input message : 'Ready to deploy?', ok: 'Yes, I am sure I want to deploy!'
					}
			}
		}
		stage('Deploy Prod') {
			agent {
				docker {
					image 'node:18-alpine'
					reuseNode true
				}
			}
            steps {
                sh '''
				npm install netlify-cli@20.1.1
                node_modules/.bin/netlify --version
				echo "Deploying in production SITE ID: $NETLIFY_SITE_ID"
				node_modules/.bin/netlify status
				node_modules/.bin/netlify deploy --dir=build --prod
				'''
            }
			
		}		
			stage(' Prod E2E Test') {
			
				environment {
				CI_ENVIRONMENT_URL = 'https://vocal-selkie-eb5066.netlify.app'
				}
				agent {
					docker {
						image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
						reuseNode true
					}
				}
				steps {
					sh '''
						npx playwright test --reporter=html
					'''
				}
				
				post {
					always {
						publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright PROD E2E Report', reportTitles: '', useWrapperFileDirectly: true])
						}
					}
				}
	}
	
}