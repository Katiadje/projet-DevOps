pipeline {
    agent any

    environment {
        IMG_NAME = 'nginx'
        DOCKER_REPO = 'test'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout SCM') {
            steps {
                git (
                    branch: 'main',
                    url: 'https://github.com/Katiadje/projet-DevOps.git'
                )
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMG_NAME} ."
                    sh "docker tag ${IMG_NAME} ${DOCKER_REPO}:${IMG_NAME}"
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh """
                        docker stop monapp || true
                        docker rm monapp || true
                        docker run -d --name monapp --hostname monapp -p 8585:80 ${IMG_NAME}
                        docker exec monapp ifconfig
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé.'
        }
    }
}
