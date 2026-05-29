pipeline {
    agent any

    parameters {
        choice(
            name: 'ENV',
            choices: ['dev', 'stage', 'prod'],
            description: 'Select Environment'
        )
    }

    environment {
        APP_NAME = "myapp"
        NAMESPACE = "${params.ENV}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                echo "Building docker image"
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
					echo "Deploying in ${ENV} environment and ${NAMESPACE}"
                '''
            }
        }
    }
}