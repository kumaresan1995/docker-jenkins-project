pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Windows') {
            steps {
                powershell 'Write-Host "Jenkins PowerShell is working"'
                powershell '$env:USERNAME'
                powershell '$env:PATH'
            }
        }

        stage('Check Docker') {
            steps {
                powershell '& "C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" --version'
                powershell '& "C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" info'
            }
        }

        stage('Build Docker Image') {
            steps {
                powershell '& "C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" build -t myapp:latest .'
            }
        }

        stage('Deploy') {
            steps {
                powershell '& "C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" rm -f myapp'
                powershell '& "C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" run -d --name myapp -p 8081:80 myapp:latest'
            }
        }

        stage('Verify') {
            steps {
                powershell '& "C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" ps'
            }
        }
    }
}