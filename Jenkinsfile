pipeline {
    agent any

    environment {
        REGISTRY_CREDS_ID = 'docker-hub-pat'
        REGISTRY_USERNAME = "prajaktasc"
        IMAGE_NAME = "bin-pythonapp"
        IMAGE_TAG = "${env.BUILD_NUMBER}"   
    }

    stages {
        stage('Initialize the environment'){
            steps {
                script {
                    echo "Initializing the pipeline"
                    sh 'docker --version'
                }
            }
        }

        stage('Build &Tag Docker Image'){
            steps {
                script{
                    sh 'docker image build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
                    sh 'docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${REGISTRY_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}'

                }
            }
        }
    }
 
}

