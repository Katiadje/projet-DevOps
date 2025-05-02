pipeline {
    agent any
    environment {
        IMAGE_NAME = 'myapp-image'
        CONTAINER_NAME = 'myapp'
        PORT = '8088'
    }
    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout SCM') {
            steps {
                git credentialsId: '61767618-7fdb-4d53-a45e-468d27e292fa', url: 'https://github.com/Katiadje/projet-DevOps'
            }
        }

        stage('Clean Docker') {
            steps {
                script {
                    sh """
                        docker stop \$CONTAINER_NAME || true
                        docker rm \$CONTAINER_NAME || true
                        docker rmi -f \$IMAGE_NAME || true
                        docker system prune -f || true
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Création du Dockerfile et construction de l'image..."
                    sh """
                        docker build -t \$IMAGE_NAME .
                    """
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    echo "Lancement du conteneur..."
                    sh """
                        docker run -d --name \$CONTAINER_NAME -p \$PORT:80 \$IMAGE_NAME || {
                            echo 'Erreur lors de l'exécution de docker run'
                            exit 1
                        }
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
