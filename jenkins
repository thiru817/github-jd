pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "mydockerhubusername/myflaskapp"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/thiru817/github-jd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Deploy') {
            steps {
                sh "docker stop myapp || true && docker rm myapp || true"
                sh "docker run -d --name myapp -p 5000:5000 ${DOCKER_IMAGE}:${BUILD_NUMBER}"
            }
        }
    }
}
