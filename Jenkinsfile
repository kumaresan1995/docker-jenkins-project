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
                powershell 'docker build -t myapp:latest .'
            }
        }

        stage('Deploy') {
            steps {
                powershell '''
                    docker rm -f myapp 2>$null
                    docker run -d --name myapp -p 8081:80 myapp:latest
                '''
            }
        }

        stage('Verify') {
            steps {
                powershell 'docker ps'
            }
        }
    }
}