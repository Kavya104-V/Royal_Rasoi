pipeline {
    agent any

    environment {
        IMAGE_NAME = 'kavyav549/royalrasoi'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Kavya104-V/Royal_Rasoi, credentialsId: 'github-creds'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                  credentialsId: 'dockerhub-creds',
                  usernameVariable: 'DOCKER_USER',
                  passwordVariable: 'DOCKER_PASS'
                )]) {
                    script {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        sh 'docker push $IMAGE_NAME'
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'kubectl apply -f k8s-deployment.yaml'
                    sh 'kubectl apply -f k8s-service.yaml'
                }
            }
        }
    }
}
