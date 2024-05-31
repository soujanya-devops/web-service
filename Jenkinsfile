pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "hello-service"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo/hello-service.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}").inside {
                        sh 'python -m unittest discover'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                withKubeConfig([credentialsId: 'aws-kubeconfig']) {
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
    }
}

