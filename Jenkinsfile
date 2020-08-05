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
            sh '''sudo chown -R 112:117 "/.npm"
node --version
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