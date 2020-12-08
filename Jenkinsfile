pipeline {
  environment {
    registry = "zwayyed00/capstone"
    registryCredential = 'dockerhub'
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

    stage('Deploy Blue Deployment') {
      steps {
	withAWS(region:'us-west-2', credentials:'aws-static') {
          sh 'kubectl apply -f Blue_Deployment.yaml'
          sh 'kubectl apply -f Blue_Svc.yaml'
          sh 'kubectl get deployments -o wide'
          sh 'kubectl get services -o wide'
	}
      }
    }
    stage('Human Approval') {
            steps {
                input "Redirect traffic to green deployment now?"
      }
    }
    
     stage('Deploy Green Deployment') {
      steps {
        withAWS(region:'us-west-2', credentials:'aws-static') {
          sh 'kubectl apply -f Green_Deployment.yaml'
          sh 'kubectl apply -f Green_Svc.yaml'
          sh 'kubectl get deployments -o wide'
          sh 'kubectl get services -o wide'
        }
      }
    }


  }
}
