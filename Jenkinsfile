pipeline {

    agent any

    environment {
        IMAGE_NAME = "gold2251/devops-app"
        CONTAINER_NAME = "devops-container"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/prudhvi2251/simple-nodejs-devops-project.git'
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

        stage('Deploy Container') {
            steps {

                sh 'docker stop $CONTAINER_NAME || true'

                sh 'docker rm $CONTAINER_NAME || true'

                sh 'docker pull $IMAGE_NAME:latest'

                sh 'docker run -d --name $CONTAINER_NAME -p 3000:3000 $IMAGE_NAME:latest'
            }
        }
    }

    post {

        success {
            echo 'CI/CD Pipeline Executed Successfully!'
        }

        failure {
            echo 'Pipeline Failed!'
        }
    }
}