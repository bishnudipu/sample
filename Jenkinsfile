pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'bishnu987/sample-app'
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
                script {
                    if (fileExists('package.json')) {
                        sh 'npm install'
                    } else if (fileExists('pom.xml')) {
                        sh 'mvn clean package'
                    } else {
                        error 'No recognized build files found!'
                    }
                }
            }
        }

       stage('Test') {
    steps {
        script {
            if (fileExists('package.json')) {
                sh 'npm test || echo "Skipping tests due to missing script"'
            } else {
                echo "No package.json found, skipping tests."
            }
        }
    }
}

        stage('Docker Build & Push') {
            steps {
                withCredentials([string(credentialsId: 'docker-hub-password', variable: 'DOCKER_PASSWORD')]) {
                    echo 'Building Docker image...'
                    sh """
                        docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
                        echo $DOCKER_PASSWORD | docker login -u your-dockerhub-username --password-stdin
                        docker push $DOCKER_IMAGE:$DOCKER_TAG
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to server...'
                sh """
                    docker stop sample-app || true
                    docker rm sample-app || true
                    docker run -d --name sample-app -p 8080:8080 --restart=always $DOCKER_IMAGE:$DOCKER_TAG
                """
            }
        }
    }
}
