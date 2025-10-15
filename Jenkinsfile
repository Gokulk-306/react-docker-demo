pipeline {
    agent any

    environment {
        IMAGE_NAME = "gokulk306/react-docker-demo"
        IMAGE_TAG = "v1"
    }

    stages {
        stage('Checkout') {
            agent any
            steps {
                checkout scm
            }
        }

        stage('Build React App') {
            agent {
                docker {
                    image 'node:18-alpine'
                    args '-u root'
                }
            }
            steps {
                sh 'npm config set cache /tmp/.npm-cache --global'
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Build Docker Image') {
            agent any
            steps {
                sh 'docker version'   // sanity check
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Push to Docker Hub') {
            agent any
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
                }
            }
        }

        stage('Remove Image from Host') {
            agent any
            steps {
                sh 'docker rmi $IMAGE_NAME:$IMAGE_TAG || true'
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }
    }
}
