pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/main']],
                          doGenerateSubmoduleConfigurations: false,
                          extensions: [[$class: 'SparseCheckoutPaths',  sparseCheckoutPaths: [[path: 'k8sPodTemplate.yaml']]]],
                          submoduleCfg: [],
                          userRemoteConfigs: [[url: 'https://github.com/MadhavSake/jenkins-cloudformation-deployment-example.git']]])
            }
        }
        stage('Build') {
            steps {
                // Add your build steps here
            }
        }
        stage('Deploy') {
            steps {
                // Add your deployment steps here
            }
        }
    }
    
    post {
        failure {
            echo 'Pipeline failed, sending notification...'
            // Add notification steps here
        }
    }
}
