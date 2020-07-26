pipeline {
  agent any
  stages {
    stage('Clean Up') {
      parallel {
        stage('Clean Up') {
          steps {
            echo 'Blue Ocean Hello World! HamiDev!!!'
          }
        }

        stage('My Parallel Stage') {
          steps {
            echo 'This is a parallel stage with Clean Up stage.'
          }
        }

      }
    }

    stage('Compile') {
      steps {
        echo 'This is the compile stage'
        echo 'This is the compile stage, step 2'
        echo 'This is the compile stage, step 3'
      }
    }

  }
}