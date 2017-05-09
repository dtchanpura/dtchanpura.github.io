pipeline {
  agent any
  stages {
    stage('environment') {
      steps {
        sh 'gem install jekyll'
      }
    }
    stage('build') {
      steps {
        sh 'jekyll --src $WORKSPACE build'
      }
    }
  }
}