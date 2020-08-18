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

    stage('Push to Dest Repo - dev') {
      when {
        branch 'dev'
      }
      steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'github_cred', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
          sh "echo ${GIT_USERNAME}"
          sh '''cd dist
ls
#make sure the repository does have the related branch. you might need to manually create all the branches needed for the jenkins like dev, qa.
git clone --single-branch --branch dev https://github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git
cp -a hamidev-mobile-dev-env/. hamidev-mobile-dev-env-angular-dest/
cd hamidev-mobile-dev-env-angular-dest
git config user.name "HamidBehnam"
git config user.email "hamid.behnam@gmail.com"
git add .
git diff --quiet && git diff --staged --quiet || git commit -am "adding the build files to the dest repo"
git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git'''
        }

        script {
          step([$class: "RundeckNotifier",
          includeRundeckLogs: true,
          jobId: "3935e4d5-044d-4011-8713-875b826a585b",
          rundeckInstance: "rundeck",
          options: """
          project_type=angular
          project_path=102
          project_branch=dev
          """,
          shouldFailTheBuild: true,
          shouldWaitForRundeckJob: true,
          tailLog: true])
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
#make sure the repository does have the related branch. you might need to manually create all the branches needed for the jenkins like dev, qa.
git clone --single-branch --branch qa https://github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git
cp -a hamidev-mobile-dev-env/. hamidev-mobile-dev-env-angular-dest/
cd hamidev-mobile-dev-env-angular-dest
git config user.name "HamidBehnam"
git config user.email "hamid.behnam@gmail.com"
git add .
git diff --quiet && git diff --staged --quiet || git commit -am "adding the build files to the dest repo"
git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git'''
        }

        script {
          step([$class: "RundeckNotifier",
          includeRundeckLogs: true,
          jobId: "3935e4d5-044d-4011-8713-875b826a585b",
          rundeckInstance: "rundeck",
          options: """
          project_type=angular
          project_path=102
          project_branch=qa
          """,
          shouldFailTheBuild: true,
          shouldWaitForRundeckJob: true,
          tailLog: true])
        }

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
cp -a hamidev-mobile-dev-env/. hamidev-mobile-dev-env-angular-dest/
cd hamidev-mobile-dev-env-angular-dest
git config user.name "HamidBehnam"
git config user.email "hamid.behnam@gmail.com"
git add .
git diff --quiet && git diff --staged --quiet || git commit -am "adding the build files to the dest repo"
git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git'''
        }

        script {
          step([$class: "RundeckNotifier",
          includeRundeckLogs: true,
          jobId: "3935e4d5-044d-4011-8713-875b826a585b",
          rundeckInstance: "rundeck",
          options: """
          project_type=angular
          project_path=102
          project_branch=master
          """,
          shouldFailTheBuild: true,
          shouldWaitForRundeckJob: true,
          tailLog: true])
        }

      }
    }

    stage('Post Build') {
      steps {
        cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable: true, deleteDirs: true, cleanupMatrixParent: true)
      }
    }

  }
  environment {
    HOME = '.'
  }
}