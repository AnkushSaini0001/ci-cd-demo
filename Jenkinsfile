pipeline {
    agent any

    environment {
        IMAGE_NAME = "docker-react-demo"
        CONTAINER_NAME = "docker-demo"
    }

    stages {

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
            echo 'Deployment Successful!'
        }

        failure {
            echo 'Deployment Failed!'
        }
    }
}

// pipeline {
//     agent any

//     environment {
//         SERVER = "192.168.167.194"
//         USER = "altruist"
//         DEPLOY_PATH = "/home/altruist/react_demo"
//     }

//     stages {

//         stage('Install Dependencies') {
//             steps {
//                 sh 'npm install'
//             }
//         }

//         stage('Build React App') {
//             steps {
//                 sh 'npm run build'
//             }
//         }

//         stage('Deploy Build') {
//             steps {
//                 sshagent(['server-ssh-key']) {
//                     sh """
//                         ssh ${USER}@${SERVER} "mkdir -p ${DEPLOY_PATH}"

//                         rsync -avz --delete \
//                             dist/ \
//                             ${USER}@${SERVER}:${DEPLOY_PATH}/
//                     """
//                 }
//             }
//         }
//     }
// }