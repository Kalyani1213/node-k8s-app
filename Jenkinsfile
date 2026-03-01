pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t <docker-username>/node-k8s-app:latest .'
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker push <docker-username>/node-k8s-app:latest'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s-deployment.yaml -n test-pipeline'
                sh 'kubectl apply -f k8s-service.yaml -n test-pipeline'
            }
        }
    }
}
