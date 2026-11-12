
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'static-website:v1'
        CONTAINER_NAME = 'static-website'
        PORT = '8080'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Pulling code from GitHub...'

                git branch: 'main',
                    url: 'https://github.com/ranjandevopsec/static-website.git'

                echo "Project Path: ${WORKSPACE}"
            }
        }

        stage('Test') {
            steps {
                echo 'Testing website files...'

                powershell '''
                    if (!(Test-Path "$env:WORKSPACE\\Dockerfile")) {
                        throw "Dockerfile not found!"
                    }
                    Write-Host "Dockerfile found successfully"
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'

                powershell '''
                    docker build -t static-website:v1 `
                    -f "$env:WORKSPACE\\Dockerfile" `
                    "$env:WORKSPACE"
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying website...'

                powershell '''
                    docker rm -f static-website 2>$null

                    docker run -d `
                    --name static-website `
                    -p 8080:80 `
                    static-website:v1
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline Successfully Completed!'
        }

        failure {
            echo 'Pipeline Failed!'
        }
    }
}
