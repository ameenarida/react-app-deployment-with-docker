pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ameenarida/react-docker-app .'
            }
        }
        stage('Push to DockerHub') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {
            sh '''
            echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
            docker push ameenarida/react-docker-app
            '''
        }
    }
}
        stage('Run Docker Container') {
            steps {
                sh 'docker stop react-container || true'
                sh 'docker rm react-container || true'
                sh 'docker run -d -p 80:80 --name react-container ameenarida/react-docker-app'
            }
        }
    }
}
