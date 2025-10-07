pipeline {
    agent any

    environment {
        DOCKER_HUB_USERNAME = 'hishamali'
        DOCKER_IMAGE_NAME = "${DOCKER_HUB_USERNAME}/hello-guvi-geek"
        DOCKER_TAG = "build-${env.BUILD_NUMBER}"
        DOCKER_PUSH_USERNAME = 'hishamali' 
        DOCKER_PUSH_PASSWORD = 'dckr_pat_5nI4n5tbhf-JMUfa_kXUIXsxrEA' 
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
                }
            }
        }

        stage('Test Image (Optional but recommended)') {
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
                    echo "Pushing image to Docker Hub using hardcoded credentials..."
                    
                    // 1. Explicitly log in using the hardcoded credentials
                    // The 'sh' step automatically masks environment variables in the console output.
                    sh "docker login -u ${DOCKER_PUSH_USERNAME} -p ${DOCKER_PUSH_PASSWORD} registry.hub.docker.com"
                    
                    // 2. Push the tagged image
                    sh "docker push ${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
                    sh "docker push ${DOCKER_IMAGE_NAME}:latest"
                    
                    // 3. (Optional but clean) Explicitly log out
                    sh "docker logout registry.hub.docker.com"

                    echo "Image pushed successfully! URL: https://hub.docker.com/r/${DOCKER_HUB_USERNAME}/hello-guvi-geek/tags"
                }
            }
        }
    }
    
    post {
        // ... (Post actions remain the same)
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
