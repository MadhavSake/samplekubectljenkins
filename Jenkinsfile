pipeline {
    agent {
        kubernetes {
            yamlFile 'k8s/k8sPodTemplate.yaml'
        }
    }

    parameters {
        string(name: 'STACK_NAME', defaultValue: 'example-stack', description: 'Enter the CloudFormation Stack Name.')
        string(name: 'PARAMETERS_FILE_NAME', defaultValue: 'example-stack-parameters.properties', description: 'Enter the Parameters File Name (Must contain file extension type *.properties)')
        string(name: 'TEMPLATE_NAME', defaultValue: 'S3-Bucket.yaml', description: 'Enter the CloudFormation Template Name (Must contain file extension type *.yaml)')
        credentials(name: 'CFN_CREDENTIALS_ID', defaultValue: '', description: 'AWS Account Role.', required: true)
        choice(
            name: 'REGION',
            choices: ['us-east-1', 'us-east-2'],
            description: 'AWS Account Region'
        )
        choice(
            name: 'ACTION',
            choices: ['create-changeset', 'execute-changeset', 'deploy-stack', 'delete-stack'],
            description: 'CloudFormation Actions'
        )
        booleanParam(name: 'TOGGLE', defaultValue: false, description: 'Are you sure you want to perform this action?')
    }

    environment {
        AWS_DEFAULT_REGION = "${REGION}"
    }

    stages {
        stage('Pull Docker Image') {
            steps {
                script {
                    // Log in to ECR
                    sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"

                    // Pull Docker image from ECR
                    sh "docker pull ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Deploy to EKS Cluster') {
            steps {
                script {
                    // Apply Kubernetes manifests
                    sh "kubectl apply -f path/to/kubernetes/manifests"
                }
            }
        }
    }
}
