pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'jekyll --source /usr/share/webapps/blog --destination /usr/share/webapps/blog-web'
      }
    }
  }
}