pipeline {
  agent {
    docker {
      image 'node'
    }

  }
  stages {
    stage('Start') {
      steps {
        sh '''echo "this is a test!"
node --version'''
      }
    }

  }
}