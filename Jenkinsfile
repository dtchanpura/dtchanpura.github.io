pipeline {
  agent any
  stages {
    stage('environment') {
      steps {
        sh 'cd $WORKSPACE && bundle install'
      }
    }
    stage('build') {
      steps {
        sh 'jekyll --src $WORKSPACE build'
      }
    }
  }
}