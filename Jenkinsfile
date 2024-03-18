

pipeline {
  agent any

  environment {
    AWS_REGION = 'us-east-1'
    DOCKER_REGISTRY = "docker.io"
  }
//Build Docker image
  stages {
        
        stage('Build and push Docker image') {

    //   steps {
    //     sh 'docker buildx build --platform linux/amd64,linux/arm64 -t danielletchonla/nissi-image:latest --push' 
    //   }
        steps {
            script {
                // Log in to Docker registry using Jenkins credentials
                    docker.withRegistry('https://${DOCKER_REGISTRY}', 'danielletchonla') {
            // Build and push Docker image
                sh 'docker build -t danielletchonla/slick-image:v1.0 .'
                sh 'docker push danielletchonla/slick-image:v1.0 '
        }
    }

        stage('Deploy to Kubernetes') {

        steps {
        eksDeploy(
          configs: ['eks/deployment.yml', 'eks/service.yml'],
          kubeconfigId: 'my-kubeconfig'
        )

      }
        }

    stage('Build') {
            steps {
                git branch: 'main', url: 'https://github.com/DanielleTchonla/CICD-with-Kubernetes-and-Jenkins.git'
            }
        }

    stage('Deploy to CloudFormation') {
      steps {
        script {
          withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'Danielle', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
            // sh "aws cloudformation deploy --stack-name newstack1 --template-file webapp --region \"${AWS_REGION}\""
            // sh "aws cloudformation deploy --stack-name App --template-file Dockerfile --region \"${AWS_REGION}\""
            sh "aws cloudformation deploy --stack-name docker --template-file Dockerfile --region \"${AWS_REGION}\""
            sh "aws cloudformation deploy --stack-name service --template-file service.yml --region \"${AWS_REGION}\""
            // sh 'kubectl apply -f deployment.yml' 
            sh "aws cloudformation deploy --stack-name deployment --template-file deployment.yml --region \"${AWS_REGION}\""
        }
      }
    }

//   post {
//     success {
//       echo 'Danielle, your Deployment succeeded!'
//     }
//     failure {
//       echo 'Mme F, your Deployment failed!'
//     }
//   }
  }}
}}


// pipeline {
//     agent any

//     stages {

//         stage('Build') {
//             steps {
//                 git 'https://github.com/DanielleTchonla/CICD-with-Kubernetes-and-Jenkins.git'
//             }
//         }
//         stage('Deploy') {
//             steps {
//                 sh 'kubectl apply -f deployment.yml'
//             }
//         }
//     }
// }
}