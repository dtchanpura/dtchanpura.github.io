pipeline {
  agent any
  stages {
    stage('environment') {
      steps {
        sh 'gem install jekyll bundler bundle'
      }
    }
    stage('build') {
      steps {
        sh '''
export PATH=$PATH:$HOME/.gem/ruby/2.4.0/bin && cd $WORKSPACE && bundle install && jekyll -s $WORKSPACE -d /usr/share/webapps/blog build'''
      }
    }
  }
}