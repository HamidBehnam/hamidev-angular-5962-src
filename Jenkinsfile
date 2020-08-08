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
        sh '''ls
cd dist
cd hamidev-mobile-dev-env
ls
git clone https://github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git
git status
'''
      }
    }

    stage('Post Build') {
      steps {
        cleanWs(cleanWhenSuccess: true)
        sh 'ls'
      }
    }

  }
  environment {
    HOME = '.'
  }
}