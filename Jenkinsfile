pipeline {
    agent any 

    environment {
        // Docker Hub credentials stored in Jenkins
        DOCKERHUB_CREDENTIALS = credentials('c5287d2c-f404-47db-a606-4ac216197454')
        IMAGE_NAME = 'amilaerandika/alpine'
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                git branch: 'main', credentialsId: 'amila-git-credentials', url: 'https://github.com/amilaerandika/git-learn.git'
            }
        }

        stage('Check Docker') {
            steps {
                sh '''
                    export PATH=$PATH:/usr/local/bin:/opt/homebrew/bin
                    echo "PATH=$PATH"
                    which docker || echo "Docker not found"
                    docker --version || echo "Docker CLI not available"
                    docker context use default || true
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                        export PATH=$PATH:/usr/local/bin:/opt/homebrew/bin
                        docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'c5287d2c-f404-47db-a606-4ac216197454') {
                        sh '''
                            export PATH=$PATH:/usr/local/bin:/opt/homebrew/bin
                            docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                        '''
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh '''
                        export PATH=$PATH:/usr/local/bin:/opt/homebrew/bin
                        docker rmi ${IMAGE_NAME}:${BUILD_NUMBER} || echo "Image already removed or not found"
                    '''
                }
            }
        }
    }
}
