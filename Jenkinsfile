pipeline {
  agent {
    node {
      label 'node_agent'
    }

  }
  stages {
    stage('Report') {
      steps {
        sh '''echo "this is for test"
echo ${PATH}
node --version'''
      }
    }

  }
}