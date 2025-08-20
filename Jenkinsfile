pipeline {
    agent any

    environment {
        IMAGE = "hariviswanathb/devops-demo"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/HariviswanathB/devops-demo.git',
                    credentialsId: 'github-creds'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push ${IMAGE}:${BUILD_NUMBER}
                      docker tag ${IMAGE}:${BUILD_NUMBER} ${IMAGE}:latest
                      docker push ${IMAGE}:latest
                    """
                }
            }
        }
    }
}

