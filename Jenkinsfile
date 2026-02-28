pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "kalyani1213/node-k8s-app:latest" // Docker Hub repo/tag
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Kalyani1213/node-k8s-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {
                // Make sure you logged in to Docker Hub in Jenkins first
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl set image deployment/node-k8s-app node-k8s-app=$DOCKER_IMAGE || kubectl apply -f k8s-deployment.yaml'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}
