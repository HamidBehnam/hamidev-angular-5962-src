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
        sh 'rm -rf node_modules'
        sh '''ls -la
rm -rf .npm
ls -la'''
        git(url: 'https://github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git', branch: 'master', credentialsId: 'github')
        sh '''cp -a dist/hamidev-mobile-dev-env/. .
rm -rf dist
git status
git branch
git add .
git commit -m "pushing some changed to the dest repo by jenkins"
git push -u origin master
git status
git branch'''
      }
    }

    stage('Post Build') {
      steps {
        sleep 10
        sh 'ls'
      }
    }

  }
  environment {
    HOME = '.'
  }
}