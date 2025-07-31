pipeline {
    agent any 

    environment {
        // Replace 'dockerhub-credentials-id' with the ID of your Docker Hub credentials stored in Jenkins
        DOCKERHUB_CREDENTIALS = credentials('c5287d2c-f404-47db-a606-4ac216197454') 
        IMAGE_NAME = 'amilaerandika/alpine' // Replace with your Docker Hub username and repository name
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                git branch: 'main', credentialsId: 'your-credential-id', url: 'https://github.com/amilaerandika/git-learn.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${env.BUILD_NUMBER} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'c5287d2c-f404-47db-a606-4ac216197454', url: 'https://hub.docker.com/repositories/amilaerandika') { // url: '' for Docker Hub
                        sh "docker push ${IMAGE_NAME}:${env.BUILD_NUMBER}"
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh "docker rmi ${IMAGE_NAME}:${env.BUILD_NUMBER}" // Remove the built image locally
                }
            }
        }
    }
}
