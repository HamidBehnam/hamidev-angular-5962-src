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
        sh 'npm run build -- --base-href /${PROJECT_CATEGORY}/${PROJECT_PATH}/'
      }
    }

    stage('Push to Dest Repo - dev') {
      when {
        branch 'dev'
      }
      steps {
          sh '''cd dist
ls
#make sure the repository does have the related branch. you might need to manually create all the branches needed for the jenkins like dev, qa.
git clone --single-branch --branch dev https://${DEST_REPO}
cp -a hamidev-mobile-dev-env/. hamidev-mobile-dev-env-angular-dest/
cd hamidev-mobile-dev-env-angular-dest
git add .
git diff --quiet && git diff --staged --quiet || git commit -am "adding the build files to the dest repo"
git push https://${GITHUB_CRED_USR}:${GITHUB_CRED_PSW}@${DEST_REPO}'''

        script {
          step([$class: "RundeckNotifier",
          includeRundeckLogs: true,
          jobId: "${RUNDECK_JOB_ID}",
          rundeckInstance: "${RUNDECK_INSTANCE}",
          options: """
          project_category=${PROJECT_CATEGORY}
          project_path=${PROJECT_PATH}
          deployment_branch=dev
          dest_repo=${DEST_REPO}
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
git clone --single-branch --branch qa https://${DEST_REPO}
cp -a hamidev-mobile-dev-env/. hamidev-mobile-dev-env-angular-dest/
cd hamidev-mobile-dev-env-angular-dest
git add .
git diff --quiet && git diff --staged --quiet || git commit -am "adding the build files to the dest repo"
git push https://${GIT_USERNAME}:${GIT_PASSWORD}@${DEST_REPO}'''
        }

        script {
          step([$class: "RundeckNotifier",
          includeRundeckLogs: true,
          jobId: "${RUNDECK_JOB_ID}",
          rundeckInstance: "${RUNDECK_INSTANCE}",
          options: """
          project_category=${PROJECT_CATEGORY}
          project_path=${PROJECT_PATH}
          deployment_branch=qa
          dest_repo=${DEST_REPO}
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
git clone https://${DEST_REPO}
cp -a hamidev-mobile-dev-env/. hamidev-mobile-dev-env-angular-dest/
cd hamidev-mobile-dev-env-angular-dest
git add .
git diff --quiet && git diff --staged --quiet || git commit -am "adding the build files to the dest repo"
git push https://${GIT_USERNAME}:${GIT_PASSWORD}@${DEST_REPO}'''
        }

        script {
          step([$class: "RundeckNotifier",
          includeRundeckLogs: true,
          jobId: "${RUNDECK_JOB_ID}",
          rundeckInstance: "${RUNDECK_INSTANCE}",
          options: """
          project_category=${PROJECT_CATEGORY}
          project_path=${PROJECT_PATH}
          deployment_branch=master
          dest_repo=${DEST_REPO}
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
    GITHUB_CRED = credentials('github_cred')
    PROJECT_CATEGORY = 'angular'
    PROJECT_PATH = '583'
    RUNDECK_JOB_ID = '3935e4d5-044d-4011-8713-875b826a585b'
    RUNDECK_INSTANCE = 'rundeck'
    DEST_REPO = 'github.com/HamidBehnam/hamidev-mobile-dev-env-angular-dest.git'
  }
}
