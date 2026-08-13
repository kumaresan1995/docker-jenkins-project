pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t myapp:latest .'
            }
        }

        stage('Deploy') {
            steps {
                bat 'docker rm -f myapp || exit 0'
                bat 'docker run -d --name myapp -p 8081:80 myapp:latest'
            }
        }

        stage('Verify') {
            steps {
                bat 'docker ps'
            }
        }
    }
}