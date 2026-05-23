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
                git 'https://github.com/Shrutikapukale/learn-jenkins-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t myapp:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh """
                helm upgrade --install ${APP_NAME} ./helm \
                  -f helm/values-${ENV}.yaml \
                  --namespace ${NAMESPACE}
                """
            }
        }
    }
}