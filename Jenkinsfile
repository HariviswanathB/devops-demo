pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "hariviswanathb/devops-demo"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/HariviswanathB/devops-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-creds') {
                        app.push()
                    }
                }
            }
        }
    }
}


