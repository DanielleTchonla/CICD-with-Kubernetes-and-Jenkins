pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        K8S_NAMESPACE = 'test'
        EKS_CLUSTER_NAME = 'nisssi-cluster'
        DEPLOYMENT_YML_PATH = 'path/to/deployment.yaml'
        SERVICE_YML_PATH = 'path/to/service.yaml'
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
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'Danielle']]) {
                        withEnv(['PATH+EXTRA=/usr/local/bin']) {
                            // Update kubeconfig for the EKS cluster (assuming AWS CLI is configured)
                            sh "aws eks --region ${AWS_DEFAULT_REGION} update-kubeconfig --name ${EKS_CLUSTER_NAME}"
                            
                            // Check if namespace exists before attempting to create it
                            sh "kubectl get namespace ${K8S_NAMESPACE} || kubectl create namespace ${K8S_NAMESPACE}"
                            
                            // Apply deployment YAML
                            sh "kubectl apply -f ${DEPLOYMENT_YML_PATH} -n ${K8S_NAMESPACE}"
                            
                            // Apply service YAML
                            sh "kubectl apply -f ${SERVICE_YML_PATH} -n ${K8S_NAMESPACE}"
                        }
                    }
                }
            }
        }
    }
}



// pipeline {
//     agent any
    
//     environment {
//         AWS_DEFAULT_REGION = 'us-east-1'
//         //KUBE_NAMESPACE = 'default' // Kubernetes namespace to deploy to
//         K8S_NAMESPACE = 'test'
//         EKS_CLUSTER_NAME = 'nisssi-cluster'
//     }
    
//     stages {
//         stage('Build') {
//             steps {
//                 sh 'docker build -t slick-image .'
//             }
//         }

//         stage('Push image') {
//             steps {
//                 withDockerRegistry([ credentialsId: "danielletchonla", url: "https://index.docker.io/v1/" ]) {
//                     sh "docker push danielletchonla/slick-image:v1.0"
//                 }
//             }
//         }

//         stage('Deploy to EKS') {
//             steps {
//                 // script {
//                 //     // Authenticate with AWS using Jenkins credentials
//                 //     withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'Danielle']]) {
//                 //         // Use kubectl command with specific context to deploy to EKS
//                 //         sh 'kubectl config set-context --current --namespace=${KUBE_NAMESPACE}'
//                 //         sh 'kubectl apply -f deployment.yaml'
//                 //         sh 'kubectl apply -f service.yaml'
//                 //     }
//                 // }
//                 script {
//                     withEnv([‘PATH+EXTRA=/usr/local/bin’]) {
//                         // Update kubeconfig for the EKS cluster (assuming AWS CLI is configured)
//                         sh “aws eks --region ${AWS_DEFAULT_REGION} update-kubeconfig --name ${EKS_CLUSTER_NAME}”
//                         // Create the namespace if it doesn’t exist
//                         sh “kubectl create namespace ${K8S_NAMESPACE} --dry-run=client -o yaml | kubectl apply -f -”
//                         // Apply deployment YAML
//                         sh “kubectl apply -f ${DEPLOYMENT_YAML_PATH} -n ${K8S_NAMESPACE}”
//                         sh “kubectl apply -f ${SERVICE_YAML_PATH} -n ${K8S_NAMESPACE}”
//           }
//         }
//             }
//         } 
//     }
// }







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


