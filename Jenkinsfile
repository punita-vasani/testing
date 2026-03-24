pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/var/www/html'
        REPO_URL   = 'https://github.com/punita-vasani/testing.git'
    }

    triggers {
        pollSCM('H/2 * * * *')   // every 2 minutes check for changes
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: "${REPO_URL}"
            }
        }

        stage('Build') {
            steps {
                echo 'Running build steps...'
                sh 'echo Build complete'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo Tests passed'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying website...'
                sh '''
                mkdir -p /var/www/html
                rm -rf /var/www/html/*
                cp -r * /var/www/html/
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
