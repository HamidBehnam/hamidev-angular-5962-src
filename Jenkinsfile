pipeline {
  agent {
    docker {
      image 'node'
    }

  }
  stages {
    stage('Pre Build') {
      parallel {
        stage('Print Info') {
          steps {
            sh '''echo Printing some info messages.
node --version
echo "Here\'s the content of the current directory"
ls'''
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