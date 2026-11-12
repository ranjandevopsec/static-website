pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Pulling code from GitHub...'
                checkout scm
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Static Website...'

                powershell '''
                if (!(Test-Path index.html)) {
                    Write-Error "index.html not found"
                    exit 1
                }
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'

                powershell '''
                docker build -t static-website:latest .
                if ($LASTEXITCODE -ne 0) { exit $LASTEXITCODE }
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Website...'

                powershell '''
                docker rm -f static-website
                docker run -d --name static-website -p 8080:80 static-website:latest

                if ($LASTEXITCODE -ne 0) { exit $LASTEXITCODE }
                '''
            }
        }
    }

    post {
        success {
            echo 'Website deployed successfully!'
        }

        failure {
            echo 'Pipeline Failed!'
        }
    }
}
