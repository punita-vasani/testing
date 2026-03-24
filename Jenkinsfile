pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/var/www/html'   // Nginx serves from here
        REPO_URL   = 'https://github.com/YOUR_USERNAME/YOUR_REPO.git'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: "${REPO_URL}",
                    credentialsId: 'github-token'
            }
        }

        stage('Build') {
            steps {
                echo 'Running build steps...'
                // If you have a build step (e.g. npm), add it here:
                // sh 'npm install && npm run build'
                sh 'echo Build complete'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // sh 'npm test'
                sh 'echo Tests passed'
            }
        }

        // ✅ THIS IS THE APPROVAL GATE
        stage('Approval') {
            steps {
                script {
                    def approver = input(
                        id: 'DeployApproval',
                        message: '🚀 Deploy to production?',
                        submitterParameter: 'APPROVER',
                        parameters: [
                            choice(
                                name: 'ACTION',
                                choices: ['Deploy', 'Abort'],
                                description: 'Choose to deploy or abort'
                            )
                        ]
                    )
                    if (approver['ACTION'] == 'Abort') {
                        error('Deployment aborted by approver.')
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to AWS EC2...'
                sh """
                    sudo cp -r ${WORKSPACE}/. ${DEPLOY_DIR}/
                    sudo chown -R www-data:www-data ${DEPLOY_DIR}
                    sudo systemctl reload nginx
                """
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
            // emailext subject: 'Deployment Successful',
            //          body: 'Your site has been deployed.',
            //          to: 'you@example.com'
        }
        failure {
            echo '❌ Pipeline failed or was aborted.'
        }
    }
}
