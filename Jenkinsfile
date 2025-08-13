pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-app:latest"
        CONTAINER_NAME = "my-app-container"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    
                    sh "docker rm -f ${CONTAINER_NAME} || true"
                    
                    sh "docker run -d --name ${CONTAINER_NAME} -p 5000:5000 ${IMAGE_NAME}"
                }
            }
        }

        stage('Verify') {
            steps {
                script {
                
                    sh "docker ps | grep ${CONTAINER_NAME}"
                }
            }
        }
    }
}

