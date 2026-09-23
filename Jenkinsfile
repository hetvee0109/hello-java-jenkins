pipeline {
    agent any

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
                bat '"C:/Users/admin/AppData/Local/Programs/DockerDesktop/resources/bin/docker.exe" build -t hello-java-jenkins:%BUILD_NUMBER% -t hello-java-jenkins:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat '"C:/Users/admin/AppData/Local/Programs/DockerDesktop/resources/bin/docker.exe" run --rm hello-java-jenkins:latest'           
		 }
        }
    }
}