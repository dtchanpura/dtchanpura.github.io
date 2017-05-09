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
        sh '''
export PATH=$PATH:$HOME/.gem/ruby/2.4.0/bin && jekyll -s $WORKSPACE -d /usr/share/webapps/blog build'''
      }
    }
  }
}