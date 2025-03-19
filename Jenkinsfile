pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-angular-app'
        CONTAINER_NAME = 'exciting_kilby'
        REPO_URL = 'https://github.com/Roopagotur/jenkins-cicd-docker.git'
        HOST_PORT = '8081'  // Changed from 8080 to avoid conflict
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    sh '''
                    if [ -d "my-angular-app" ]; then
                        echo "Directory exists. Removing..."
                        rm -rf my-angular-app
                    fi
                    git clone ${REPO_URL} my-angular-app
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${IMAGE_NAME} my-angular-app'
                }
            }
        }

        stage('Stop and Remove Existing Container') {
            steps {
                script {
                    sh '''
                    # Check and stop any container using the same port
                    CONTAINER_ID=$(docker ps -q --filter "publish=${HOST_PORT}")
                    if [ ! -z "$CONTAINER_ID" ]; then
                        echo "Stopping container using port ${HOST_PORT}..."
                        docker stop $CONTAINER_ID
                        docker rm $CONTAINER_ID
                    fi
                    
                    # Also stop our specific container if it exists
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    '''
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh '''
                    docker run -d -p ${HOST_PORT}:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}
                    echo "Container started on port ${HOST_PORT}"
                    '''
                }
            }
        }
    }
    
    post {
        failure {
            echo "Pipeline failed! Check the logs for details."
        }
        success {
            echo "Pipeline completed successfully! App is running on port ${HOST_PORT}"
        }
    }
}
