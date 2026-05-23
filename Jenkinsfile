pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {

                git branch: 'main',
                url: 'https://github.com/Shrutikapukale/learn-jenkins-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo Building Application'
            }
        }

        stage('Deploy Dev') {
            steps {
                sh 'echo Deploying to Dev'
            }
        }
    }
}