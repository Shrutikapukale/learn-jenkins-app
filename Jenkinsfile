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
                git branch: 'main',
                url 'https://github.com/Shrutikapukale/learn-jenkins-app.git'
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