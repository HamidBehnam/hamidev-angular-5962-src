pipeline {
  agent {
    docker {
      image 'maven'
    }

  }
  stages {
    stage('Report') {
      steps {
        sh '''echo "this is for test"
echo ${PATH}
echo ${M2_HOME}'''
      }
    }

  }
}