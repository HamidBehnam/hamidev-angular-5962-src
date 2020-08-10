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
        sh '''git ls-remote --heads
cd dist
git clone https://github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git
cd hamidev-mobile-dev-env-angular-dest
git ls-remote --heads
#git config user.name "HamidBehnam"
#git config user.email "hamid.behnam@gmail.com"
#git status
#git branch
#git add .
#git commit -m "pushing some changed to the dest repo by jenkins"
#git push -u origin master
#git status
#git branch'''
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