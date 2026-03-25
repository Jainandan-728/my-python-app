pipeline {
    agent any

    environment {
        // Define your image name here
        DOCKER_IMAGE = "my-python-app-jenkins"
    }

    stages {
        stage('Checkout') {
            steps {
                // This pulls the code from your GitHub
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                script {
                    // This builds the Docker image locally on the Jenkins server
                    sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_ID} ."
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo "Running basic smoke tests..."
                    // Example: Check if the python file exists
                    sh "test -f app.py"
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    echo "Cleaning up old images..."
                    // Optional: remove the image to save disk space after build
                    sh "docker rmi ${DOCKER_IMAGE}:${env.BUILD_ID}"
                }
            }
        }
    }

    post {
        always {
            echo "Build finished!"
        }
        success {
            echo "Deployment Ready!"
        }
        failure {
            echo "Something went wrong. Check the logs."
        }
    }
}
