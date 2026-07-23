pipeline {
    agent any

    environment {
        IMAGE_NAME = "docker-react-demo"
        CONTAINER_NAME = "docker-demo"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker stop ${CONTAINER_NAME} || true'
                sh 'docker rm ${CONTAINER_NAME} || true'
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p 5173:80 \
                    ${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }

        failure {
            echo "Deployment Failed!"
        }
    }
}