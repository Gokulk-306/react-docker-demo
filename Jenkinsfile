pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            args '-u root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        IMAGE_NAME = "gokulk306/react-docker-demo"  // Change this to your Docker Hub username
        IMAGE_TAG = "v1"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
                }
            }
        }

        stage('Remove Image from EC2') {
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
