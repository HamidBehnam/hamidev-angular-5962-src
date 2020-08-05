pipeline {
  agent {
    docker {
      image 'node'
    }
  }
  environment {
    HOME = '.'
  }
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            sh '''node --version
echo "The Build Stage!"'''
          }
        }

        stage('Clearing') {
          steps {
            sh '''rm -rf node_modules
rm -rf dist'''
          }
        }

        stage('Install npm dependencies') {
          steps {
            sh 'npm install'
          }
        }

      }
    }

  }
}
