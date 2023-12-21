pipeline {
    agent any

    environment{
        REGION = 'ap-northeast-2'
        EKS_API = 'https://7a5e757692ef53832af1aecc6481b691.sk1.ap-northeast-2.eks.amazonaws.com/'
        EKS_CLUSTER_NAME = 'Eks-Cluster'
        EKS_JENKINS_CREDENTIAL_ID = 'kubectl-deploy-credentials'
        ECR_PATH = '194453983284.dkr.ecr.ap-northeast-2.amazonaws.com'
        ECR_IMAGE = 'reca-ecr'
        AWS_CREDENTIAL_ID = 'AWSCredentials'
    }
    stages {

        stage('Clone Repository'){
            steps {
                checkout scm
            }
        }

        stage('Docker Build'){
            steps {
                script {
                    docker.withRegistry("https://${ECR_PATH}", "ecr:${REGION}:${AWS_CREDENTIAL_ID}"){
                        image = docker.build("${ECR_PATH}/${ECR_IMAGE}")
                    }
                }
            }
        }

        stage('Push to ECR'){
            steps {
                script {
                    docker.withRegistry("https://${ECR_PATH}", "ecr:${REGION}:${AWS_CREDENTIAL_ID}"){
                        image.push("v${env.BUILD_NUMBER}")
                    }
                }
            }
        }

        stage('CleanUp Images'){
            steps {
                sh"""
                docker rmi ${ECR_PATH}/${ECR_IMAGE}:v$BUILD_NUMBER
                docker rmi ${ECR_PATH}/${ECR_IMAGE}:latest
                export SHOPPING_VER=v${env.BUILD_NUMBER}
                """
            }
        }

        stage('Deploy to k8s'){
            steps {
                script {
                    withKubeConfig([credentialsId: "${EKS_JENKINS_CREDENTIAL_ID}",
                                    serverUrl: "${EKS_API}",
                                    clusterName: "${EKS_CLUSTER_NAME}"]){
                                        sh "sed 's/\\\$SHOPPING_VER/v${env.BUILD_NUMBER}/g' service.yaml > output.yaml"
                                        sh "aws eks --region ${REGION} update-kubeconfig --name ${EKS_CLUSTER_NAME}"
                                        sh "kubectl config use-context arn:aws:eks:ap-northeast-2:194453983284:cluster/Eks-Cluster"
                                        sh "kubectl apply -f output.yaml"
                                        sh "rm output.yaml"

                                        sh "sed 's/\\\$SHOPPING_VER/v${env.BUILD_NUMBER}/g' ingress.yaml > output-ingress.yaml"
                                        sh "kubectl apply -f output-ingress.yaml"
                                        sh "rm output-ingress.yaml"

                    }
                }
            }
        }

    }
}