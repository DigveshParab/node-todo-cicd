pipeline {
    agent any

    environment {
        IMAGE_NAME = "todo-node-app"
        CONTAINER_NAME = "todo-node-app-container"
        UNIQUE_TAG = "${env.BUILD_ID}"
        FULL_IMAGE_NAME = "${IMAGE_NAME}:${UNIQUE_TAG}"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image with a unique tag
                    sh "docker build -t ${FULL_IMAGE_NAME} ."
                }
            }
        }
        
        stage('Deploy Docker Container') {
            steps {
                script {
                    // Stop and remove any existing container with the same name
                    sh '''
                    if [ $(docker ps -aq -f name=${CONTAINER_NAME}) ]; then
                        docker stop ${CONTAINER_NAME}
                        docker rm ${CONTAINER_NAME}
                    fi
                    '''
                    
                    // Run the new container
                    sh "docker run -d --name ${CONTAINER_NAME} -p 8000:8000 ${FULL_IMAGE_NAME}"
                }
            }
        }
    }

    post {
        cleanup {
            script {
                // Remove old images to free up space
                sh "docker image prune -f"
            }
        }
    }
}

