pipeline {
    agent any
    
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials-id')
        DOCKER_HUB_USERNAME = 'hishamali'
        DOCKER_IMAGE_NAME = "${DOCKER_HUB_USERNAME}/hello-guvi-geek"
        DOCKER_TAG = "build-${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    echo "Checking out code from GitHub..."
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building docker image..."
                    docker.build("${DOCKER_IMAGE_NAME}:${DOCKER_TAG}", "-f Dockerfile .")
                }
            }
        }

        stage('Test Image (Optional but recommended)') {
            steps {
                script {
                    echo "Running container for quick test..."
                    def app = docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_TAG}").run('-d')
                    try {
                        sleep 5
                        echo "Image built and container started successfully."
                    } finally {
                        app.stop()
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    echo "Pushing image to Docker Hub..."
                    def image = docker.image("${DOCKER_IMAGE_NAME}")
                    
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_HUB_CREDENTIALS) {
                        image.push(DOCKER_TAG)
                        image.push('latest')
                    }
                    echo "Image pushed successfully! URL: https://hub.docker.com/r/${DOCKER_HUB_USERNAME}/hello-guvi-geek/tags"
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline execution finished.'
        }
        success {
            echo 'Pipeline succeeded! Check Docker Hub for the new image.'
        }
        failure {
            echo 'Pipeline failed! Check the console output for errors.'
        }
    }
}
