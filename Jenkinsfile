pipeline {
    agent any

    environment {
        DOCKER_HUB_USERNAME = 'hishamali'
        DOCKER_IMAGE_NAME   = "${DOCKER_HUB_USERNAME}/hello-guvi-geek"
        DOCKER_TAG          = "build-${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out code from GitHub..."
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building docker image..."
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_TAG} -f Dockerfile ."
                    sh "docker tag ${DOCKER_IMAGE_NAME}:${DOCKER_TAG} ${DOCKER_IMAGE_NAME}:latest"
                }
            }
        }

        stage('Test Image (Optional)') {
            steps {
                script {
                    echo "Running container for quick test..."
                    def containerId = sh(returnStdout: true, script: "docker run -d ${DOCKER_IMAGE_NAME}:${DOCKER_TAG}").trim()
                    try {
                        sleep 5
                        echo "Image built and container started successfully."
                    } finally {
                        sh "docker stop ${containerId}"
                        sh "docker rm -f --volumes ${containerId}"
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    echo "Pushing image to Docker Hub using Jenkins credentials..."

                    withDockerRegistry([ credentialsId: 'DOCKER_HUB_CREDENTIALS', url: '' ]) {
                        sh "docker push ${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
                        sh "docker push ${DOCKER_IMAGE_NAME}:latest"
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
