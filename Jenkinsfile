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
        sh 'docker build --tag=zwayyed00/capstone:v1 .'
      }
    }
    
    stage('Push Docker Image to Dockerhub') {
      steps {
        withDockerRegistry([url: "", credentialsId: "dockerhub"]) {
          sh 'docker push $registry:v1'
        }
      }
    }

    stage('Set Current Context') {
      steps {
        withAWS(region:'us-west-2', credentials:'aws-static') {
          sh 'kubectl config use-context arn:aws:eks:us-west-2:946751356397:cluster/capstone'
        }
      }
    }


  }
}
