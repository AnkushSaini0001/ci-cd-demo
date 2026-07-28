pipeline {
    agent any

    environment {
        SERVER = "192.168.167.194"
        USER = "altruist"
        DEPLOY_PATH = "/home/altruist/react_demo"
    }

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy Build') {
            steps {
                sshagent(['server-ssh-key']) {
                    sh """
                        ssh ${USER}@${SERVER} "mkdir -p ${DEPLOY_PATH}"

                        rsync -avz --delete \
                            dist/ \
                            ${USER}@${SERVER}:${DEPLOY_PATH}/
                    """
                }
            }
        }
    }
}