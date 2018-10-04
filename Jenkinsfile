pipeline {
  agent any
  stages {
    stage('Setup') {
      steps {
        sh 'echo "Versions: "'
        sh 'chef --version'
        sh 'rubocop --version'
        sh 'foodcritic --version'
        sh 'echo "Updating Berkshelf: "'
        sh 'if [ ! -f Berksfile.lock ]; then berks install; else berks update; fi;'
      }
    }
    stage('Acceptance Testing') {
      parallel {
        stage('rubocop') {
          steps {
            sh 'echo "Starting chefstyle (rubocop): "'
            sh 'rubocop --color'
          }
        }
        stage('foodcritic') {
          steps {
            sh 'echo "Starting foodcritic: "'
            sh 'foodcritic .'
          }
        }
      }
    }
    stage('Test Kitchen') {
      steps {
        sh 'if [ ! -f Berksfile.lock ]; then berks install; else berks update; fi; kitchen test -d always --color'
      }
    }
  }
  post {
    success {
      echo 'Knife upload here'

    }

    failure {
      echo 'The build failed'

    }

  }
}