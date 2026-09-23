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
                bat 'docker build -t hello-java-jenkins:%BUILD_NUMBER% -t hello-java-jenkins:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat 'docker run --rm hello-java-jenkins:latest'
            }
        }
    }
}