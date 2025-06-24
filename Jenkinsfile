pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'nametona/sample-app'
        DOCKER_TAG = 'latest'
        REGISTRY_CREDENTIALS = 'docker-hub-credential'
    }

    triggers {
        githubPush() // Auto build saat push GitHub
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker image...'
                withDockerRegistry([credentialsId: "${REGISTRY_CREDENTIALS}", url: '']) {
                    sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                echo 'Deploying Docker container...'
                sh '''
                docker stop sample-app || true
                docker rm sample-app || true
                docker run -d --name sample-app -p 8080:80 $DOCKER_IMAGE:$DOCKER_TAG
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Success!'
        }
        failure {
            echo '❌ Deployment Failed!'
        }
    }
}
