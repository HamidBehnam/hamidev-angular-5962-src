pipeline {
  agent {
    docker {
      image 'node'
    }

  }
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            sh '''node --version
echo "The Build Stage!"'''
          }
        }

        stage('Clearing') {
          steps {
            sh '''#rm -rf node_modules
#rm -rf dist

rm -r /var/www/demo.hamidev.com/public_html/angular/102
mkdir /var/www/demo.hamidev.com/public_html/angular/102'''
          }
        }

        stage('Install npm dependencies') {
          steps {
            sh 'npm install'
          }
        }

      }
    }

    stage('Compile') {
      steps {
        sh 'npm run build -- --base-href /angular/102/'
      }
    }

    stage('Pre Deploy') {
      steps {
        sh '''rm -r /var/www/demo.hamidev.com/public_html/angular/102
mkdir /var/www/demo.hamidev.com/public_html/angular/102'''
      }
    }

    stage('Deploy') {
      steps {
        sh '''cp -a /var/lib/jenkins/workspace/_mobile-dev-env-angular-2_master/dist/hamidev-mobile-dev-env/. /var/www/demo.hamidev.com/public_html/angular/102/
'''
      }
    }

  }
  environment {
    HOME = '.'
  }
}