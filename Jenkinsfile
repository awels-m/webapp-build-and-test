pipeline {
    agent any

    environment {
        IMAGE_NAME = "theawels/myapp:latest"  // Corrected DockerHub repo name
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Checking out your repository...'
                checkout scm
            }
        }

        stage('Build Application') {
            steps {
                echo 'Verifying index.html exists...'
                sh '''
                    if [ -f "index.html" ]; then
                        echo "index.html found, build successful."
                    else
                        echo "index.html missing, build failed."
                        exit 1
                    fi
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image: ${env.IMAGE_NAME}"
                    sh "docker build -t ${env.IMAGE_NAME} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo "Running Docker container from image: ${env.IMAGE_NAME}"
                    // Changed from 8080 to 8090 because 8080 is used by Jenkins
                    sh "docker run -d -p 8090:80 ${env.IMAGE_NAME}"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',  // This ID must match the one you saved in Jenkins
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $IMAGE_NAME
                    '''
                }
            }
        }
    }
}

