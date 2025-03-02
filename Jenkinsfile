pipeline {
    agent any
    
    environment {
        IMAGE_NAME = "your-docker-image"  // Change this to your Docker image name
        CONTAINER_NAME = "your-container" // Change this to your container name
        DOCKER_CREDENTIALS_ID = "your-docker-credentials-id" // Jenkins credentials for Docker
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/bishnudipu/sample.git'  // Replace with your repo URL
            }
        }

        stage('Build') {
            steps {
                script {
                    echo "Building application..."
                    sh 'npm install'  // Use 'mvn package' for Java, or 'go build' for Golang, etc.
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo "Running tests..."
                    sh 'npm test'  // Replace with the command to run your tests
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    echo "Building and pushing Docker image..."
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                    withDockerRegistry([credentialsId: DOCKER_CREDENTIALS_ID, url: ""]) {
                        sh "docker push ${IMAGE_NAME}:latest"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                    sh "docker run -d --name ${CONTAINER_NAME} -p 80:80 ${IMAGE_NAME}:latest"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline execution finished."
        }
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
