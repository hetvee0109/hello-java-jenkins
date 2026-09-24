pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hetvee114/hello-java-jenkins'
        DOCKER_PATH  = 'C:/Users/admin/AppData/Local/Programs/DockerDesktop/resources/bin/docker.exe'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Java') {
            steps {
                bat 'javac Hello.java'
            }
        }

        stage('Test Java') {
            steps {
                bat 'java Hello'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '"%DOCKER_PATH%" build -t %DOCKER_IMAGE%:%BUILD_NUMBER% -t %DOCKER_IMAGE%:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat '"%DOCKER_PATH%" run --rm %DOCKER_IMAGE%:latest'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    bat '''
                        echo %DOCKER_PASSWORD% | "%DOCKER_PATH%" login -u %DOCKER_USERNAME% --password-stdin
                        if %ERRORLEVEL% NEQ 0 exit /b %ERRORLEVEL%

                        "%DOCKER_PATH%" push %DOCKER_IMAGE%:%BUILD_NUMBER%
                        if %ERRORLEVEL% NEQ 0 exit /b %ERRORLEVEL%

                        "%DOCKER_PATH%" push %DOCKER_IMAGE%:latest
                        if %ERRORLEVEL% NEQ 0 exit /b %ERRORLEVEL%

                        "%DOCKER_PATH%" logout
                    '''
                }
            }
        }
    }
}