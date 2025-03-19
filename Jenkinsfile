pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-angular-app'
        CONTAINER_NAME = 'exciting_kilby'
        REPO_URL = 'https://github.com/Roopagotur/jenkins-cicd-docker.git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    sh 'git clone ${REPO_URL} my-angular-app'
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
                    docker run -d -p 8080:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}
                    '''
                }
            }
        }
    }
}
