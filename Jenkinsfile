pipeline {
  agent any

  environment {
    IMAGE = "hariviswanathb/devops-demo"   // replace with your Docker Hub username
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build Docker image') {
      steps {
        sh '''
          docker build -t $IMAGE:$BUILD_NUMBER .
          docker tag $IMAGE:$BUILD_NUMBER $IMAGE:latest
        '''
      }
    }

    stage('Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh '''
            echo "$PASS" | docker login -u "$USER" --password-stdin
            docker push $IMAGE:$BUILD_NUMBER
            docker push $IMAGE:latest
            docker logout || true
          '''
        }
      }
    }
  }

  post {
    always {
      sh 'docker image prune -f || true'
    }
  }
}
