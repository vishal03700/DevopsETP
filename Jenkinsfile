pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'vishal03700/devopsetp'
        IMAGE_TAG = '1.0.0'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/vishal03700/DevopsETP.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:${IMAGE_TAG}")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        dockerImage.push('latest')
                        dockerImage.push("${IMAGE_TAG}")
                    }
                }
            }
        }
        stage('Deploy Container') {
            steps {
                script {
                    // Stop the old container if it's running
                    sh 'docker ps -q --filter "name=devopsetp-container" | grep -q . && docker stop devopsetp-container || true'
                    // Run the new container
                    sh 'docker run --rm --name devopsetp-container -d -p 3000:3000 vishal03700/devopsetp:latest'
                }
            }
        }
    }
}
