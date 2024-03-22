pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        K8S_NAMESPACE = 'default'
        EKS_CLUSTER_NAME = 'xxx-cluster'
        DEPLOYMENT_YML_PATH = 'deployment.yml'
        SERVICE_YML_PATH = 'service.yml'
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





