pipeline {
    agent any 

    environment {
        IMAGE_NAME = 'gokulk306/react-task'
    }

    stages {
        stage('clone the repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Gokulk-306/Jenkins-guvi.git'
            }
        }
    }
}