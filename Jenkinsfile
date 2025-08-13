pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-app:latest"
		CONTAINER_NAME = "my-app-${env.BRANCH_NAME}"
        ECR_REPO = "992382545251.dkr.ecr.us-east-1.amazonaws.com/yuvaly-repo:latest"
        AWS_REGION = "us-east-1"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Tag for ECR') {
            steps {
                script {
                    sh "docker tag ${IMAGE_NAME} ${ECR_REPO}"
                }
            }
        }

        stage('Login to ECR') {
            steps {
                script {
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin 992382545251.dkr.ecr.${AWS_REGION}.amazonaws.com"
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    sh "docker push ${ECR_REPO}"
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

        stage('Verify Container') {
            steps {
                script {
                    sh "docker ps | grep ${CONTAINER_NAME}"
                }
            }
        }
    }
}

