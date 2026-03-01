pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "Kalyani/node-k8s-app:latest"  // Replace with your Docker Hub username
        K8S_NAMESPACE = "test-pipeline"
    }

    stages {

        stage('Checkout') {
            steps {
                // Get code from GitHub
                git branch: 'main', url: 'https://github.com/Kalyani1213/node-k8s-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {
                // Login to Docker Hub (ensure you have credentials added in Jenkins)
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh "docker push $DOCKER_IMAGE"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Apply deployment and service YAMLs
                sh "kubectl apply -f k8s-deployment.yaml -n $K8S_NAMESPACE"
                sh "kubectl apply -f k8s-service.yaml -n $K8S_NAMESPACE"
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'curl $(minikube service my-k8s-app-service -n $K8S_NAMESPACE --url)'
            }
        }
    }
}
