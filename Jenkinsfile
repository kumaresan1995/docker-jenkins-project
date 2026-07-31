pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning source code...'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f myapp || true
                docker run -d --name myapp -p 8081:80 myapp:latest
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'docker ps'
            }
        }
    }
}