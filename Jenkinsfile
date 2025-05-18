pipeline {
    agent any

    environment {
        IMAGE_NAME = 'kavyav549/restaurant-site:latest'
         KUBECONFIG = 'C:\\ProgramData\\Jenkins\\.kube\\config'

          
    }


    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/Kavya104-V/Royal_Rasoi.git', credentialsId: 'github-creds'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Windows-specific environment variable expansion for bat
                    bat "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                  credentialsId: 'docker-cred',
                  usernameVariable: 'DOCKER_USER',
                  passwordVariable: 'DOCKER_PASS'
                )]) {
                    script {
                        // Use bat to log in to Docker Hub
                        bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                        bat "docker push ${IMAGE_NAME}"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Use bat to run kubectl commands
                    bat 'kubectl apply -f k8s-deployment.yaml --validate=false'
                    bat 'kubectl apply -f k8s-service.yaml --validate=false'
                }
            }
        }
    }
}
