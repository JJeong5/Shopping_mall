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
        DOCKERHUB_USERNAME = 'jjeong5'
        DOCKERHUB_REPO = 'jjeong5/jenkins'
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
                    image = docker.build("${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}")
                }
            }
        }
        stage('Push to Docker Hub'){
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        image.push("v${env.BUILD_NUMBER}")
                    }
                }
            }
        }
        stage('CleanUp Images'){
            steps {
                sh"""
                docker rmi ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:v$BUILD_NUMBER
                docker rmi ${DOCKERHUB_USERNAME}/${DOCKERHUB_REPO}:latest
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