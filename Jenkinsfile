pipeline {
    agent any
    environment {
        PROJECT_ID = 'docker'
        CLUSTER_NAME = 'jenkins'
        LOCATION = 'us-central-1a'
        CREDENTIALS_ID = 'kubernetes'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build image') {
            steps {
                script {
                    app = docker.build("zetzo/pipeline:${env.BUILD_ID}")
                }
            }
        }
        stage('Deploy to K8s') {
            steps {
                echo "Deployment started ..."
                sh 'ls -ltr'
                sh 'pwd'
                sh "sed -i \"s/pipeline:latest/pipeline:class: 'KubernetesEngineBuilder', projectId: '\${PROJECT_ID}', clusterName: '\${CLUSTER_NAME}', location: '\${LOCATION}', manifestPattern: 'deployment.yaml', credentialsId: '\${CREDENTIALS_ID}', verifyDeployments: true/g\" deployment.yaml"
            }
        }
    }
}
