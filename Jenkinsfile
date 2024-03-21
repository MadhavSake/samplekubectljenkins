pipeline {
    agent {
        kubernetes {
            yamlFile 'k8sPodTemplate.yaml'
        }
    }

    stages {
        stage('Pull Docker Image') {
            steps {
                script {
                    // Log in to ECR
                    sh "aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin 106036894611.dkr.ecr.eu-central-1.amazonaws.com"

                    // Pull Docker image from ECR
                    sh "docker pull 106036894611.dkr.ecr.eu-central-1.amazonaws.com/eureka-server:latest"
                }
            }
        }

        stage('Deploy to EKS Cluster') {
            steps {
                script {
                    // Apply Kubernetes manifests
                    sh "kubectl apply -f deployment.yaml"
                }
            }
        }
    }
}
