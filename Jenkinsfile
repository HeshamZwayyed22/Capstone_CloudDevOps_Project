pipeline {
  agent any
  stages {
    stage('Linting Dockerfile') {
      steps {
        sh '''hadolint Dockerfile
'''
        sh 'tidy -q -e index.html'
      }
    }

  }
}