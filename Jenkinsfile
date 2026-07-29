// pipeline {
//     agent any

//     environment {
//         IMAGE_NAME = "docker-react-demo"
//         CONTAINER_NAME = "docker-demo"
//     }

//     stages {

//         stage('Build Docker Image') {
//             steps {
//                 sh 'docker build -t ${IMAGE_NAME}:latest .'
//             }
//         }

//         stage('Stop Old Container') {
//             steps {
//                 sh 'docker stop ${CONTAINER_NAME} || true'
//                 sh 'docker rm ${CONTAINER_NAME} || true'
//             }
//         }

//         stage('Run New Container') {
//             steps {
//                 sh '''
//                 docker run -d \
//                     --name ${CONTAINER_NAME} \
//                     -p 5173:80 \
//                     ${IMAGE_NAME}:latest
//                 '''
//             }
//         }
//     }

//     post {
//         success {
//             echo 'Deployment Successful!'
//         }

//         failure {
//             echo 'Deployment Failed!'
//         }
//     }
// }

pipeline {
    agent any

    environment {
        IMAGE_NAME = "react-demo"
        CONTAINER_NAME = "react-demo"

        SERVER = "192.168.167.194"
        USER = "altruist"

        REMOTE_IMAGE = "/tmp/react-demo.tar"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t ${IMAGE_NAME}:latest .
                '''
            }
        }

        stage('Save Docker Image') {
            steps {
                sh '''
                    docker save -o react-demo.tar ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Copy Image to Server') {
            steps {
                sshagent(['server-ssh-key']) {
                    sh '''
                        scp -o StrictHostKeyChecking=no react-demo.tar ${USER}@${SERVER}:${REMOTE_IMAGE}
                    '''
                }
            }
        }

        stage('Deploy on Server') {
            steps {
                sshagent(['server-ssh-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ${USER}@${SERVER} "
                            docker load -i ${REMOTE_IMAGE} &&
                            docker stop ${CONTAINER_NAME} || true &&
                            docker rm ${CONTAINER_NAME} || true &&
                            docker run -d \
                                --name ${CONTAINER_NAME} \
                                -p 80:80 \
                                --restart unless-stopped \
                                ${IMAGE_NAME}:latest
                        "
                    '''
                }
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