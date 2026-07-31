pipeline {
    agent any

    environment {
        IMAGE_NAME = "docker-react-demo"
        CONTAINER_NAME = "docker-demo"
        TEST_SERVER = "ankush@172.28.56.74"
        APP_DIR = "/home/ankush/ci-cd-demo"
    }

    stages {
        stage('Deploy to Test Server') {
            steps {
                sh """
ssh -o StrictHostKeyChecking=no ${TEST_SERVER} << EOF
set -ex

cd ${APP_DIR}

echo "===== WHOAMI ====="
whoami

echo "===== PWD ====="
pwd

echo "===== GIT ====="
git pull

echo "===== BUILD ====="
docker build -t ${IMAGE_NAME}:latest .

echo "===== STOP ====="
docker stop ${CONTAINER_NAME} || true
docker rm ${CONTAINER_NAME} || true

echo "===== RUN ====="
docker run -d \
  --name ${CONTAINER_NAME} \
  -p 5173:80 \
  ${IMAGE_NAME}:latest

echo "===== DONE ====="
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