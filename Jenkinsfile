pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t slick-image .' 
      }
    }

    stage('Push image') {
      steps {
        withDockerRegistry([ credentialsId: "danielletchonla", url: "https://index.docker.io/v1/" ]) {
          sh "docker push danielletchonla/slick-image:latest"
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
