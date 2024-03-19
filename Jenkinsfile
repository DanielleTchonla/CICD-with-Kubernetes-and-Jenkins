pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        KUBE_NAMESPACE = 'default' // Kubernetes namespace to deploy to
    }
    
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

        stage('Deploy to EKS') {
            steps {
                script {
                    // Authenticate with AWS using Jenkins credentials
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'eks-jenkins']]) {
                        // Use kubectl command with specific context to deploy to EKS
                        sh 'kubectl config set-context --current --namespace=${KUBE_NAMESPACE}'
                        sh 'kubectl apply -f deployment.yaml'
                        sh 'kubectl apply -f service.yaml'
                    }
                }
            }
        } 
    }
}




// pipeline {
//   agent any
//       environment {
//         AWS_DEFAULT_REGION = 'us-east-1'
//     }
//   stages {
//     stage('Build') {
//       steps {
//         sh 'docker build -t slick-image .' 
//       }
//     }

//     stage('Push image') {
//       steps {
//         withDockerRegistry([ credentialsId: "danielletchonla", url: "https://index.docker.io/v1/" ]) {
//           sh "docker push danielletchonla/slick-image:v1.0"
//         }
//       }
//     }

//     stage('Deploy to EKS') {
//       steps {
//           script {
//               // Authenticate with AWS using Jenkins credentials
//               withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'danielle']]) {
//               // Use kubectl command to deploy to EKS
//                   sh 'kubectl apply -f deployment.yaml'
//                   sh 'kubectl apply -f service.yaml'
//                     }
//                 }
        
        
//       }
//     }
//   }
// }


