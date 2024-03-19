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
          sh "docker push danielletchonla/slick-image:v1.0"
        }
      }
    }

    stage('Deploy') {
      steps {
        // sh 'kubectl apply -f deployment.yml'
        // sh 'kubectl apply -f service.yml'
        sh 'kubectl apply --validate=false -f deployment.yml' // Temporarily disable validation
        sh 'kubectl apply --validate=false -f service.yml' // Temporarily disable validation
        
      }
    }
  }
}
