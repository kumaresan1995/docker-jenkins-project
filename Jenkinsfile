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

        stage('Test Docker Image') {
            steps {
                powershell '''
                    docker run -d --name myapp-test -p 8082:80 myapp:latest
                    Start-Sleep -Seconds 5
                    $response = curl.exe -s -o NUL -w "%{http_code}" http://localhost:8082
                    Write-Host "HTTP Status: $response"

                    if ($response -ne "200") {
                        docker logs myapp-test
                        docker rm -f myapp-test
                        exit 1
                    }

                    docker rm -f myapp-test
                    Write-Host "Application test passed"
                '''
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
                powershell '''
                    docker ps
                    $response = curl.exe -s -o NUL -w "%{http_code}" http://localhost:8081
                    Write-Host "Deployment HTTP Status: $response"

                    if ($response -ne "200") {
                        exit 1
                    }

                    Write-Host "Deployment verification passed"
                '''
            }
        }
    }
}