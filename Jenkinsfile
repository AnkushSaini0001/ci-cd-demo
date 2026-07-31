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
        IMAGE_NAME = "docker-react-demo"
        CONTAINER_NAME = "docker-demo"
        TEST_SERVER = "ankush@172.28.56.74"
        APP_DIR = "/home/ankush/ci-cd-demo"
    }

    stages {

        // stage('Deploy to Test Server') {
        //     steps {
        //         sh """
        //         ssh -o StrictHostKeyChecking=no ${TEST_SERVER} '
        //             cd ${APP_DIR}

        //             git pull

        //             docker build -t ${IMAGE_NAME}:latest .

        //             docker stop ${CONTAINER_NAME} || true
        //             docker rm ${CONTAINER_NAME} || true

        //             docker run -d \
        //                 --name ${CONTAINER_NAME} \
        //                 -p 5173:80 \
        //                 ${IMAGE_NAME}:latest
        //         '
        //         """
        //     }
        // }
        stage('Deploy to Test Server') {
    steps {
        sh """
        ssh -o StrictHostKeyChecking=no ${TEST_SERVER} << 'EOF'
        set -ex

        cd ${APP_DIR}

        pwd
        ls -la

        git pull

        docker build -t ${IMAGE_NAME}:latest .

        docker stop ${CONTAINER_NAME} || true
        docker rm ${CONTAINER_NAME} || true

        docker run -d \
          --name ${CONTAINER_NAME} \
          -p 5173:80 \
          ${IMAGE_NAME}:latest
        EOF
        """
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