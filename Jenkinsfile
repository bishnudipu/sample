pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'your-dockerhub-username/sample-app'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/bishnudipu/sample.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'npm install'  // If using Node.js
                sh 'mvn clean package' // If using Java (Maven)
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test'  // If using Node.js
                sh 'mvn test'  // If using Java (Maven)
            }
        }

        stage('Docker Build & Push') {
            steps {
                echo 'Building Docker image...'
                sh """
                    docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
                    docker login -u bishnu987 -p Bishnu987@
                    docker push $DOCKER_IMAGE:$DOCKER_TAG
                """
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to server...'
                sh 'docker run -d -p 8080:8080 $DOCKER_IMAGE:$DOCKER_TAG'
            }
        }
    }
}
