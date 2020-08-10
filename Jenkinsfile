pipeline {
    agent {
      docker {
        image 'node'
      }
    }
  stages {
    stage('prepare .env') {
      steps {
        withCredentials([string(credentialsId: 'github_cred_text', variable: 'SECRET')]) {
          sh "echo ${SECRET}"
        }
      }
    }
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
  }
  environment {
    HOME = '.'
  }
}


// pipeline {
//   agent {
//     docker {
//       image 'node'
//     }
//
//   }
//   stages {
//     stage('Pre Build') {
//       parallel {
//         stage('Print Info') {
//           steps {
//             sh '''node --version
// ls'''
//           }
//         }
//
//         stage('Clearing') {
//           steps {
//             sh '''rm -rf node_modules
// rm -rf dist'''
//           }
//         }
//
//         stage('Install npm dependencies') {
//           steps {
//             sh 'npm install'
//           }
//         }
//
//       }
//     }
//
//     stage('Build') {
//       steps {
//         sh 'npm run build -- --base-href /angular/102/'
//       }
//     }
//
//     stage('Push to Dest Repo') {
//       steps {
//         sh '''ls
// touch something.html
// git status
// git branch
// git add .
// git commit -m "adding a test file by jenkins"
// git push -u origin master
// git status'''
//       }
//     }
//
//     stage('Post Build') {
//       steps {
//         sleep 20
//         sh 'ls'
//       }
//     }
//
//   }
//   environment {
//     HOME = '.'
//   }
// }
