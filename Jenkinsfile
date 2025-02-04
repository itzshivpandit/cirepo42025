pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/itzshivpandit/cirepo42025.git'
        BRANCH = 'main'
        APP_NAME = 'carvilla-site'
        CONTAINER_NAME = 'carvilla-container'
        SERVER_IP = '10.111.1.249'
        IMAGE_NAME = 'carvilla-site:latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    sh 'rm -rf cirepo42025 || true'
                    sh "git clone -b ${BRANCH} ${REPO_URL}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                    cd cirepo42025
                    sudo docker build -t ${IMAGE_NAME} .
                    """
                }
            }
        }

        stage('Stop and Remove Old Container') {
            steps {
                script {
                    sh """
                    sudo docker stop ${CONTAINER_NAME} || true
                    sudo docker rm ${CONTAINER_NAME} || true
                    """
                }
            }
        }

        stage('Run New Container') {
            steps {
                script {
                    sh """
                    sudo docker run -d -p 8181:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful! Your site is live at http://${SERVER_IP}:8181"
        }
        failure {
            echo "Deployment failed! Check the logs for errors."
        }
    }
}
