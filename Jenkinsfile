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

    stage('Build') {
      steps {
        sh 'npm run build -- --base-href /angular/102/'
      }
    }

    stage('Push to Dest Repo') {
      steps {
        sh '''withCredentials([string(credentialsId: \'github_cred_text\', variable: \'SECRET\')]) {
        echo "My secret text is \'${SECRET}\'"
    }'''
        }
      }

      stage('Post Build') {
        steps {
          sleep 20
          sh 'ls'
        }
      }

    }
    environment {
      HOME = '.'
    }
  }