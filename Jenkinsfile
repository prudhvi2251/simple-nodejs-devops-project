pipeline {

    agent any

    environment {
        IMAGE_NAME = "gold2251/devops-app"
    }

    stages {

        stage('Clone Code') {
            steps {
            
                git branch: 'main', url: 'https://github.com/prudhvi2251/simple-nodejs-devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {

                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'

                    sh 'docker push $IMAGE_NAME:latest'
                }
            }
        }
    }
}