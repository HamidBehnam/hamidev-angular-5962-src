pipeline {
  agent {
    docker {
      image 'node'
    }

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

    stage('Compile') {
      steps {
        sh 'npm run build -- --base-href /angular/102/'
      }
    }

  }
  environment {
    HOME = '.'
  }
}