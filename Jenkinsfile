pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t nissi .' 
      }
    }

    stage('Push image') {
      steps {
        withDockerRegistry([ credentialsId: "danielletchonla", url: "https://hub.docker.com/repository/docker/danielletchonla/nissi/" ]) {
          bat "docker push danielletchonla/nissi:latest"
        }
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
