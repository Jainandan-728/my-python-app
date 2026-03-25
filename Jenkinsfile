pipeline {
    agent any

    environment {
        
        DOCKER_IMAGE = "my-python-app-jenkins"
    }

    stages {
        stage('Checkout') {
            steps {
                
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                script {
                    
                    sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_ID} ."
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo "Running basic smoke tests..."
                    
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
