pipeline {
  agent any
  stages {
    stage('Clean Ups') {
      parallel {
        stage('Clean Up 1') {
          steps {
            echo 'Blue Ocean Hello World! HamiDev!!!'
          }
        }

        stage('Clean Up 2') {
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