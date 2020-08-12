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

    stage('prepare .env') {
      steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'github_cred', usernameVariable: 'username', passwordVariable: 'passvar')]) {
          sh "echo ${username}"
          sh '''cd dist
ls
git clone https://github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git
ls
cp -a hamidev-mobile-dev-env/. hamidev-mobile-dev-env-angular-dest/
#cd hamidev-mobile-dev-env-angular-dest
ls
git add .
git commit -m "adding the build files to the dest repo"
git push https://${username}:${passvar}@github.com/HamidBehnam/hamidev-mobile-dev-env-angular.git'''
        }

      }
    }

  }
  environment {
    HOME = '.'
  }
}