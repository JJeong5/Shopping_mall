pipeline {
    agent any

    environment{
        REGION = 'ap-northeast-2'
        EKS_API = 'https://9BCA0CEB26E00BE8AD75E92BADC9CF5C.gr7.ap-northeast-2.eks.amazonaws.com'
        EKS_CLUSTER_NAME = 'EksCluster'
        EKS_JENKINS_CREDENTIAL_ID = 'kubectl-deploy-credentials'
        ECR_PATH = '194453983284.dkr.ecr.ap-northeast-2.amazonaws.com'
        ECR_IMAGE = 'shopping_mall'
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
                """
            }
        }
        stage('Deploy to k8s'){
            steps {
                script {
                    withKubeConfig([credentialsId: "${EKS_JENKINS_CREDENTIAL_ID}",
                                    serverUrl: "${EKS_API}",
                                    clusterName: "${EKS_CLUSTER_NAME}"]){
                        sh "sed 's/IMAGE_VERSION/v${env.BUILD_ID}/g' service.yaml > output.yaml"
                        sh "aws eks --region ${REGION} update-kubeconfig --name ${EKS_CLUSTER_NAME}"
                        sh "kubectl apply -f output.yaml"
                        sh "rm output.yaml"
                    }
                }
            }
        }
    }
}