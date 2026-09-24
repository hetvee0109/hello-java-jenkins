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

        stage('Docker Logout (clear stale sessions)') {
            steps {
                // Clears any leftover cached login (e.g. from manual testing on this machine)
                // so the push below can only succeed using the Jenkins credential, not a stale session.
                bat '"%DOCKER_PATH%" logout || exit 0'
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
                        powershell -Command "Write-Host ('DOCKER_USERNAME=' + $env:DOCKER_USERNAME); Write-Host ('DOCKER_PASSWORD length=' + $env:DOCKER_PASSWORD.Length)"

                        "%DOCKER_PATH%" login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%
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