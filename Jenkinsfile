pipeline {
  agent {
    docker {
      image 'node'
    }

  }
  stages {
    stage('prepare .env') {
      steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'github_cred', usernameVariable: 'username', passwordVariable: 'passvar')]) {
          sh "echo ${username}"
          sh '''touch something.html
git add .
git commit -m "adding a test file"
git push -u origin master'''
        }

      }
    }

    stage('Pre Build') {
      parallel {
        stage('Print Info') {
          steps {
            sh '''node --version
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

  }
  environment {
    HOME = '.'
  }
}