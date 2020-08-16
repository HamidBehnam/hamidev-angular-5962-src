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
        step([$class: "RundeckNotifier",
              includeRundeckLogs: true,
              jobId: "3935e4d5-044d-4011-8713-875b826a585b",
              nodeFilters: "",
              options: """
                       PARAM_1="Hamid1"
                       PARAM_2="Hamid2"
                       PARAM_3="Hamid3"
                       """,
              rundeckInstance: "Default",
              shouldFailTheBuild: true,
              shouldWaitForRundeckJob: true,
              tags: "",
              tailLog: true])
      }
    }

    stage('Push to Dest Repo - master') {
      when {
        branch 'master'
      }
      steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'github_cred', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
          sh "echo ${GIT_USERNAME}"
          sh '''cd dist
ls
git clone https://github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git
cd hamidev-mobile-dev-env-angular-dest
git checkout master
git pull
cd ..
cp -a hamidev-mobile-dev-env/. hamidev-mobile-dev-env-angular-dest/
cd hamidev-mobile-dev-env-angular-dest
git config user.name "HamidBehnam"
git config user.email "hamid.behnam@gmail.com"
git add .
git diff --quiet && git diff --staged --quiet || git commit -am "adding the build files to the dest repo"
git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git'''
        }

      }
    }

    stage('Push to Dest Repo - qa') {
      when {
        branch 'qa'
      }
      steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'github_cred', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
          sh "echo ${GIT_USERNAME}"
          sh '''cd dist
ls
git clone https://github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git
cd hamidev-mobile-dev-env-angular-dest
git checkout -b qa
#this condition is added to make sure we're running git pull if the branch remote exists. No need to do this for the master since already exists.
(git ls-remote --quiet --heads | grep -q qa && true || false) && git pull origin qa
cd ..
cp -a hamidev-mobile-dev-env/. hamidev-mobile-dev-env-angular-dest/
cd hamidev-mobile-dev-env-angular-dest
git config user.name "HamidBehnam"
git config user.email "hamid.behnam@gmail.com"
git add .
git diff --quiet && git diff --staged --quiet || git commit -am "adding the build files to the dest repo"
git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git'''
        }

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
