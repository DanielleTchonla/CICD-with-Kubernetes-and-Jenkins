pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t nissi .' 
      }
    }
    stage('Push') {
      steps {
        sh 'docker push nissi'
      }
    }
    stage('Deploy') {
      steps {
        sh 'kubectl apply -f deployment.yml'
        sh 'kubectl apply -f service.yml'
      }
    }
  }
}
