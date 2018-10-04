pipeline {
  agent any
  stages {
    stage('Setup') {
      steps {
        sh 'echo "Versions: "'
        sh 'chef --version'
        sh 'chef exec rubocop --version'
        sh 'chef exec foodcritic --version'
        sh 'echo "Updating Berkshelf: "'
        sh 'if [ ! -f Berksfile.lock ]; then chef exec berks install; else chef exec berks update; fi;'
      }
    }
    stage('Acceptance Testing') {
      parallel {
        stage('rubocop') {
          steps {
            sh 'echo "Starting chefstyle (rubocop): "'
            sh 'chef exec rubocop --color'
          }
        }
        stage('foodcritic') {
          steps {
            sh 'echo "Starting foodcritic: "'
            sh 'chef exec foodcritic .'
          }
        }
      }
    }
    stage('Test Kitchen') {
      steps {
        sh 'if [ ! -f Berksfile.lock ]; then chef exec berks install; else chef exec berks update; fi; chef exec kitchen test -d always --color'
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
