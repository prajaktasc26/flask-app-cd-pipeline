pipeline {
    agent any

    environment {
        REGISTRY_CREDS_ID = 'docker-hub-pat'
        SSH_HOST_CREDS_ID  = 'remote-server-ssh'
        REGISTRY_USERNAME = "prajaktasc"
        IMAGE_NAME = "bin-pythonapp"
        IMAGE_TAG = "${env.BUILD_NUMBER}"  
               SSH_TARGET_HOST    = '98.81.228.171'
        SSH_TARGET_USER    = 'ec2-user'
        CONTAINER_NAME     = 'flask-app'
        HOST_PORT          = '8080'
        CONTAINER_PORT     = '8080' 
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
        stage('Push image to registry'){
            steps{
                withCredentials([usernamePassword(credentialsId: "${REGISTRY_CREDS_ID}",
                usernameVariable: 'USER', passwordVariable: 'PASS')]){
                    sh "echo ${PASS} | docker login -u ${USER} --password-stdin"
                    sh "docker image push ${REGISTRY_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
                
            }
        }
        
        stage('Deploy to remote docker host') {
            steps {
                sshagent(["${SSH_HOST_CREDS_ID}"]) {
                    withCredentials([
                        usernamePassword(
                            credentialsId: "${REGISTRY_CREDS_ID}",
                            usernameVariable: 'USER',
                            passwordVariable: 'PASS'
                        )
                    ]) {
                        sh '''
                            set -eu

                            ssh -o StrictHostKeyChecking=no \
                                ${SSH_TARGET_USER}@${SSH_TARGET_HOST} \
                                "docker login -u '${USER}' --password-stdin" <<EOF
                                ${PASS}
                             EOF

                            ssh -o StrictHostKeyChecking=no \
                                ${SSH_TARGET_USER}@${SSH_TARGET_HOST} \
                                "docker pull ${REGISTRY_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG} &&
                                if docker ps -aq -f name=^${CONTAINER_NAME}$ | grep -q .; then
                                    docker rm -f ${CONTAINER_NAME}
                                fi &&
                                docker run -d \
                                    --name ${CONTAINER_NAME} \
                                    -p ${HOST_PORT}:${CONTAINER_PORT} \
                                    --restart always \
                                    ${REGISTRY_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG} &&
                                docker logout"
                        '''
                    }
                }
            }
        }

 }
post {
    always {
        sh 'docker logout || true'
        sh 'docker image rm "${REGISTRY_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}" "${IMAGE_NAME}:${IMAGE_TAG}" || true'
    }
}
 
}

