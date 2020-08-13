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
        withCredentials(bindings: [usernamePassword(credentialsId: 'github_cred', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
          sh "echo ${GIT_USERNAME}"
          sh '''cd dist
ls
git clone https://github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git
ls
cp -a hamidev-mobile-dev-env/. hamidev-mobile-dev-env-angular-dest/
cd hamidev-mobile-dev-env-angular-dest
ls
git config user.name "HamidBehnam"
git config user.email "hamid.behnam@gmail.com"
git add .
git diff --quiet && git diff --staged --quiet || git commit -am "adding the build files to the dest repo"
git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git'''
        }

      }
    }

  }
  environment {
    HOME = '.'
  }
}
