pipeline {
  agent {
    docker {
      image 'node'
    }

  }
  stages {
    stage('Start') {
      steps {
        sh '''echo "this is for test"
echo ${PATH}
echo node --version'''
      }
    }

  }
}