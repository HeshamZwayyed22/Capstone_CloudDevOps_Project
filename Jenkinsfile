pipeline {
  environment {
    registry = "zwayyed00/capstone"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Linting') {
      steps {
        sh '''hadolint Dockerfile
'''
        sh 'tidy -q -e index.html'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build --tag=zwayyed00/capstone .'
      }
    }
    
    stage('Push Docker Image to Dockerhub') {
      steps {
        withDockerRegistry([url: "", credentialsId: "dockerhub"]) {
          sh 'docker push $registry:v1'
        }
      }
    }

    stage('Push Docker Image to Dockerhub') {
      steps {
        dockerNode(image: 'capstone', credentialsId: 'dockerhub')
      }
    }

  }
}
